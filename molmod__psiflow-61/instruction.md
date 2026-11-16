# Issue

**Title:** Create symlinks into psiflow_internal directory

## Description

Turn off with  `make_symlinks: False` in psiflow config.
Also removed option to supply custom path (fixed to `psiflow_internal` \)

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `0e5f68c3e87c`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.