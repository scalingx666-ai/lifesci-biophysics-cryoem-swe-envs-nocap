# Issue

**Title:** Accept conformer id in utilities

## Description

Small quality of life improvement to accept the target conformer ID as an optional argument in several utility functions.

Also adds `read_sdf_mols_by_name`, similar to `read_sdf` except returning a dict mapping name to mol.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `eb7d8d7d09b3`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.