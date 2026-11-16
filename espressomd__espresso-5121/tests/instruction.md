# Issue

**Title:** Parallelize P3M force assignment

## Description

Description of changes:
- execute `AssignForces` with OpenMP (partial fix for #4959)

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `8f33f31b75dd`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.