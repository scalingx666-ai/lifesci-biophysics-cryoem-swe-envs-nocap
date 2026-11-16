# Issue

**Title:** Model downloads: once per machine, never half-visible

## Description

## Summary

SciLink downloads its model weights on first use: SAM for particle segmentation, the DCNN ensemble for atom finding, and CIFs from the Crystallography Open Database. When several analyses need the same missing file at once (parallel branches, best-of-N candidates, series replays), they used to download it into the same place together. An analysis could then load a half-written SAM checkpoint, and the DCNN ensemble could be read while it was still being unpacked. Now the first analysis downloads the file, the others wait and use it, and a file or folder appears only once it is complete.

This is stage 0 of the agent-swarms proposal (#676). It fixes something today's fan-out can already hit on a machine that has not downloaded the weights yet.

## Technical description

**New: `scilink/utils/file_lock.py`.**
- `path_lock(path)` is an exclusive `flock` on `<path>.lock`, held across threads and processes. `flock` on separately opened descriptors conflicts within one process too.
- On a system without `fcntl` it falls back to a per-process `threading.Lock`.
- It follows the same pattern as the script bank's domain lock, without its re-entrancy bookkeeping. The knowledge-base PR stacked on this one uses it too.

**New: `scilink/utils/download.py`.**
- `fetch_to(url, dest)`:
  - streams into a unique same-directory `mkstemp` file;
  - checks `Content-Length`;
  - sets umask permissions (`mkstemp` creates files `0600`, which a shared cache must not have);
  - publishes with `os.replace`;
  - raises `DownloadError` and removes the temp file on any failure.
- `download_once(url, dest)`: `path_lock(dest)` around fetch-if-missing, with the existence check repeated once the lock is held.

**Callers.**
- `ParticleAnalyzer._ensure_checkpoint` replaces `urllib.request.urlretrieve(url, checkpoint_path)` (straight to the final path) with `download_once`.
- The COD CIF cache does the same.
- `atomistic_model_manager.get_or_download_atomistic_model`:
  - locks the target folder and re-checks under the lock;
  - `_download_url` uses `fetch_to`;
  - `_download_and_extract_model` downloads to a unique zip, extracts into a unique staging folder and publishes it with one `os.rename`, so a reader sees no ensemble or the whole ensemble;
  - an incomplete earlier folder is moved aside to `.<name>.<rand>.stale`, not deleted;
  - the published folder gets umask permissions (`mkdtemp` makes `0700`).

**Tests.** `tests/test_download_once.py` runs against a real, slow local HTTP server that counts requests.
- 8 threads get 1 request.
- 3 spawned processes get 1 request.
- A watcher never sees a partial file.
- A short transfer fails and leaves nothing behind.
- A failed download releases the lock.
- A downloaded file gets umask permissions.
- A cached file is not fetched again.
- Concurrent SAM analyzers fetch once.
- Concurrent DCNN requests fetch once, and a watcher never sees 1–4 of 5 ensemble members.
- An incomplete DCNN folder is moved aside with its contents kept.

The SAM and DCNN race tests fail on `main`: the SAM checkpoint is fetched more than once, the watcher catches a partial ensemble, and the incomplete folder is not preserved.
The existing `test_atomistic_model_cache.py` and `test_sam_device.py` pass unchanged.

**Live verification** (Bedrock `us.anthropic.claude-opus-4-8`, real network, empty model cache):
- SAM `vit_b`: two processes at once, one download started, 375 MB checkpoint with mode `0644`, loads with `torch.load` (314 tensors).
- DCNN ensemble: two processes at once, one 770 MB download from the release asset (28 s), both returned the same path, all 5 `atomnet3*.tar` members, folder mode `0755`, no `.zip`/`.extract`/`.stale` leftovers.
- An autonomous image analysis (a 256×256 crop of the ferroelectric demo, DCNN detector requested) then ran on the published cache: `success` in 179 s, `detect_atoms_dcnn` used, no second download.
- Full offline suite: the same 57 failures/errors as `main` (environment noise), none new; 10 new tests pass.

A first attempt ran two full-size (1024×1024) analyses of this demo at once on an 8 GB laptop: their generated scripts reached 6–8 GB each and the machine froze. That is why the live check uses a crop, and it is recorded in the proposal (#676) as the case for a pre-launch capacity plan.

**Not changed.** A SAM checkpoint that an older SciLink left truncated at its final path is still trusted: there is no checksum to check it against.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `b988c7cdab5a`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.