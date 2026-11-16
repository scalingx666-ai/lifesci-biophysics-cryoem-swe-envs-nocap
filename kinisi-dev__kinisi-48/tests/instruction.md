# Issue

**Title:** Move to cov_nearest

## Description

Move the hand made find nearest covariance matrix that is positive semi-definite in favour of the `statsmodels` version.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `53cced519e71`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.