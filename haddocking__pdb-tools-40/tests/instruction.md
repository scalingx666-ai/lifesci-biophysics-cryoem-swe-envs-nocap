# Issue

**Title:** Considers isalhum in pdb_selchain

## Description

Considers chain options to be valid when str.isalnum() is True.

Solves #39

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `eb1baf7bd69e`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.