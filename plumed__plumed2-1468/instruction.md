# Issue

**Title:** Update the GROMACS 2026 patch so one patch fits 2026.0-2026.4

## Description

##### Description

GROMACS 2026.4 (tagged 2026-10-02) has moved out from under the baselines stored in
`patches/gromacs-2026.0.diff`, and the patch now produces a **silently broken tree** against it. This
adds a second directory for 2026.4 onwards and leaves the existing one frozen for 2026.0–2026.3.

### What drifted — enumerated, not sampled

I checked all twelve stored baselines against v2026.4 rather than only the one that failed:

| stored baseline | drift vs v2026.4 | result today |
|---|---|---|
| `applied_forces/plumed/plumedforceprovider.cpp` | 5 lines | **hunk #3 FAILS** |
| `mdlib/sim_util.cpp` | **27 lines** | applies, but **only via `-F 5` fuzz** |
| `mdrun/md.cpp` | 5 lines | applies, but **only via fuzz** |
| the other nine | byte-identical | clean |

`git log v2026.0..v2026.4 -- <file>` attributes them:

- `sim_util.cpp` — "Make sure non-local NB kernel waits for shift copy", "Fix DLB FLOP accounting for NBNXM"
- `md.cpp` — "Preserve staged PME forces for trajectory output", "Fix and re-enable MdrunTerminationTest with GPU Graphs"
- `plumedforceprovider.cpp` — one commit: **"Pass a communicator address to PLUMED's setMPIComm"**, which is my own GROMACS MR !6133

So the drift is spread across unrelated GROMACS work — and the largest of it, `sim_util.cpp`, has
nothing to do with PLUMED. But the file that actually *fails* drifted for exactly one reason:
**GROMACS adopted the `setMPIComm` address-of fix natively in 2026.4.** PLUMED's own version of that
modification is therefore redundant from 2026.4 on, which is precisely why the new directory drops it.

The two fuzz-passing files matter even though they pass. Fuzz is how a stale baseline stays invisible:
it absorbs drift that happens to miss a hunk, right up until drift lands inside one. Those two are one
upstream edit away from behaving like `plumedforceprovider.cpp` does now.

### Why the failure is silent, and why that is the serious part

`patches/patch.sh` applies the directory format at lines 316–317:

```sh
diff -U 5 "$diff/$bckfile" "$diff/$file" --label="$bckfile" --label="$file" |
patch -u -l -b -F 5 -N --suffix=.preplumed "$file"
```

There is no `set -e` and `patch`'s exit status is never examined, so a rejected hunk is reported only
on stderr and the run reports success. Reproduction against a pristine `git archive v2026.4` tree:

```
patching file ./src/gromacs/applied_forces/plumed/plumedforceprovider.cpp
Hunk #3 FAILED at 117.
Hunk #4 succeeded at 178 (offset 3 lines).
1 out of 4 hunks FAILED -- saving rejects to .../plumedforceprovider.cpp.rej
```

and afterwards, in the patched tree:

```
grep -c "GREX setMPIIntercomm"  ->  0
grep -c "GREX init"             ->  0
```

Hunk #3 carries `setNumOMPthreads`, the **entire GREX/multisim block**, and the `setMPIComm` fix. With
the GREX block gone, `PlumedMain::multi_sim_comm` keeps its default `MPI_COMM_SELF`, whose `Get_size()`
is 1 — so in `mdrun -multidir … -plumed` every replica concludes it is replica 0 of 1 and multi-replica
biases act on a single replica. The build succeeds, the run completes, the exit status is 0, and the
answer is wrong.

This was reproduced twice independently: once by extracting v2026.4 with `git archive` and replaying
the loop above, and once by replaying `patch.sh` directly against the shipped tag.

### Why a second directory rather than refreshing the first

Refreshing `gromacs-2026.0.diff`'s baselines in place would fix 2026.4 **and break 2026.0–2026.3**,
whose files still match the current baselines exactly (verified for all four tags). No single stored
pair can satisfy both: PLUMED's `setMPIComm` modification must be **present** for 2026.0–2026.3, where
the bug is still there, and **absent** for 2026.4, where GROMACS now has it. A static diff cannot
express "apply here, not there".

So `gromacs-2026.0.diff` stays exactly as it is and keeps serving 2026.0–2026.3, and this new
directory serves 2026.4 onwards.

**Two things I want to flag rather than let you find:**

1. **This departs from long-standing practice, and I would rather say so.** As far as I can tell PLUMED
   has never shipped two directories for one GROMACS series — I checked every same-series case back to
   2018, and the convention has been to rename as the baseline moves (`461b0c7a6`, "gmx2020.6 to 2020.7
   just renamed nothing relevant for plumed changed", and `5b31d29b9` which added `gromacs-2024.3.diff`
   while deleting `2024.2.diff`). I think 2026 is a genuine exception because the series splits
   mid-flight: a fix PLUMED supplies was adopted upstream *between* point releases, so the two halves
   of the series need different patches. If you would rather keep one directory per series, the
   alternative is to rename to `gromacs-2026.4.diff` and drop support for 2026.0–2026.3 — I did not
   want to make that call unilaterally.

2. **The directory name is the only guidance users get.** `patch.sh` has no version check (line 179 is
   just `PS3="Choose the best matching code/version:"`), so naming is by *lower bound*: someone on
   2026.2 should pick `2026.0`, someone on 2026.5 should pick `2026.4`. Worth a word in the docs if you
   agree with the two-directory approach.

### What is in here

All twelve baselines are taken from v2026.4, so nothing relies on fuzz. Verified on a fresh
`git archive v2026.4` tree: every hunk applies with no `FAILED` and no `with fuzz`, and afterwards
`GREX setMPIIntercomm` = 1, native `plumedComm` = 2, `setNumOMPthreads` = 1.

`patches/gromacs-2026.4.config` is byte-identical to `gromacs-2026.0.config`; its feature list (GREX,
OpenMP thread count, ENERGY CV) still describes what this patch provides.

##### Target release

I would like my code to appear in release __v2.10__

##### Type of contribution

- [X] changes to code or doc authored by PLUMED developers, or additions of code in the core or within the default modules
- [ ] changes to a module not authored by you
- [ ] new module contribution or edit of a module authored by you

##### Copyright

- [X] I agree to transfer the copyright of the code I have written to the PLUMED developers or to the author of the code I am modifying.

##### Tests

- [ ] I added a new regtest or modified an existing regtest to validate my changes.
- [ ] I verified that all regtests are passed successfully on [GitHub Actions](https://github.com/plumed/plumed2/actions).

<!-- Both boxes left unticked deliberately. The regtest suite does not exercise the stored GROMACS
patches, which would need a patched GROMACS build, so there is no regtest to add that would cover
this. I will tick the second box once CI has run on this PR.

If you would find it useful, the check I used is worth considering for the patching machinery itself:
after patching, assert that every line a stored pair intends to add is present in the patched tree.
It is derived from the patch rather than hand-maintained, and unlike an exit-status or .rej check it
does not false-positive on already-applied hunks (patch -N writes a .rej and exits 1 for those too --
applying the 2026 patch twice to a correct 2026.3 tree leaves 12 .rej files on a perfectly good
tree). Happy to open that separately if there is interest. -->

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `a7654be01cfd`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.