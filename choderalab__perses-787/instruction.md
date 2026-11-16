# Issue

**Title:** update CI to test release candidate

## Description

I removed the part we commented out for the release candidate test and also added some cases to the matrix, let me know if this looks correct @jaimergp :smile:

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `b5d65a690776`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.