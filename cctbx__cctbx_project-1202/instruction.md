# Issue

**Title:** Switch xtb_manager to engine=inertial for exact $fix on the native backend

## Description

`xtb_manager` always runs xTB's native backend (it never uses `--spinpol`/tblite). There `inertial` (FIRE) optimises in Cartesians and holds a `$fix` scaffold exactly, while `lbfgs`/`rf` work in approximate normal coordinates and silently drift frozen atoms under large force (reporting convergence anyway), injecting noise into the optimised region. Switch to `engine=inertial` and update the test/docstrings. (Only on the tblite backend that `--spinpol` forces is `lbfgs` the exact one.)

Verified on xtb 6.7.1: native + `inertial` gives 0.000 Å frozen-atom drift.
Tests pass, including live-xTB runs with `PHENIX_XTB` set.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `69fdee3b26b5`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.