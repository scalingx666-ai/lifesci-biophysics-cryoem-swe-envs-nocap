# Issue

**Title:** SAMS improvements

## Description

* SAMS now properly restores `t0` and `stage`, which should correctly persist the stage across phase switches and resumes
* Added SAMS `logZ_history` plot to notebook reports
* SAMS `self._stage` is now an integer to more easily store this in storage (0 is initial, 1 is asymptotic phase)
* Suppress some verbose outputs
* If an asymptotically optimal stage is present in a SAMS simulation, the initial stage is discarded to equilibration and automatic equilibration detection is applied to the asymptotically optimal weight adjustment region only.
* Jupyter notebook reports now show SAMS weight convergence if present

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `c61ee9ca6a8a`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.