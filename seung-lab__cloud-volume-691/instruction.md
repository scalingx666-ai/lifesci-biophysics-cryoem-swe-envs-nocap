# Issue

**Title:** fix: use explicit spawn context for all multiprocessing primitives

## Description

`Queue`, `Lock`, and `ProcessPoolExecutor` were using different mp contexts. `mp.Queue()` and `mp.Lock()` used the default context (fork on Linux), while `ProcessPoolExecutor` used an explicit spawn context. Passing fork-context primitives to spawn-context workers raises `RuntimeError`.

This worked inconsistently because `cloudfiles` calls `multiprocessing.set_start_method("spawn", force=True)` as a side effect during its first download. If that ran before `parallel_execution`, the default context was already `spawn` and it matched. Otherwise it crashed.

Fix: create all primitives from the same explicit `spawn` context.

Note the following subtlety:

If a CV has a mesh directory, during `CloudVolume.__getitem__`, it calls `PrecomputedMeshMetadata.fetch_info() → cache.download_json() → cloudfiles.get() → cloudfiles.parallel_execute → set_start_method("spawn", force=True)`. This happens before `parallel_execution` runs - so it works fine; it only manifests for volumes without meshes.

Also, `CloudFiles` _really_ should not force change the global `start_method` for a download.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `0cb0af5c40da`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.