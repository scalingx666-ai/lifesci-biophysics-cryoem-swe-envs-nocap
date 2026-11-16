# Issue

**Title:** Complete first-step domain exchanges for inactive inputs

## Description

## Summary

Complete first-step domain exchanges for initialized inputs even when those inputs are inactive on that step. `shareData()` starts these transfers to populate fixed masses and charges, but `waitData()` previously waited only for active inputs. Clearing `firststep` with that exchange incomplete leaves later atomic actions with stale data.

This targets v2.10, the oldest maintained branch with this input-action exchange path. v2.9 uses the older Atoms exchange implementation.

## Validation

- Independent native v2.10 MPI build passed.
- New `basic/rt-dd-firststep-inactive-mpi` regression passed.
- Existing `basic/rt65-mpi` and `basic/rt74` regressions passed.
- Bundled astyle and diff whitespace checks passed.

The regression activates an atomic action after an initially inactive step and checks that the initialized data are available. No tolerances were relaxed.

## CI follow-up

The independently reviewable Python/OpenMP regression lifetime fix #1470 is now included in this branch. It retains the loaded kernel through process shutdown; three two-thread runs matched the original reference. The one-thread calculation had already passed. No thread-count reduction or tolerance change is used. GNU Autoconf download timeouts in `codecheck` occur before source checks and remain an external CI limitation.

## Regression follow-up

Included the accepted `rt-protein` shutdown fix #1473, the `rt-grid` follow-up #1477, and the timestep-test initialization fix accepted in #1464. The Python configs pass three direct two-thread runs per case with byte-identical reference output; the existing build reports Python unavailable to the native runner. The timestep regression passes through the native runner with poisoned automatic-variable initialization against both v2.10 and master kernels. No reference values, tolerances or thread counts were changed. The Autoconf download failure has a separate upstream CI proposal in #1474.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `a7654be01cfd`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.