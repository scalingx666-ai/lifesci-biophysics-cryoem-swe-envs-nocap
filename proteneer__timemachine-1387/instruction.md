# Issue

**Title:** Protocol overlap utils

## Description

Splitting out utilities from  , omitting the optimization method, but retaining the utilities for producing an approximate function `overlap(lam_a, lam_b) -> scalar` from a `bisection_result`

(see commit  for detailed omission)

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `3d0ca7e20d9a`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.