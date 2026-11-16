# Issue

**Title:** cli.py: fix error message with correct recommendation

## Description

The current recommendation does not work, also specifying the data_dir is not necessary.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `47cfa5ee8bae`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.