# Issue

**Title:** Fix metrics computation

## Description

RMSE computation was per batch -> pass Metrics object to logger

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `21becbb93ce3`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.