# Issue

**Title:** Make sure overlap can't be negative

## Description

- Update to make sure the overlap can't be negative due to rounding
- Revert to old method of computing the overlap which seems to be more stable

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `12a0cc2df1a3`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.