# Issue

**Title:** Double Max Force Norm threshold

## Description

* A few cases have failed minimization due to the threshold, but do run stably. Now that  is in place, unstable simulations will be caught eventually.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `5556d0e8fd32`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.