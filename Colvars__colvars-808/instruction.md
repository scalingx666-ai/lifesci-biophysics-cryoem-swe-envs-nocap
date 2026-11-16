# Issue

**Title:** fix: fix the calculation of bias factor with off-grid hills

## Description

When the hill to be deposited is off-grid, we cannot obtain the energy from the grid as that would be an out-of-bound memory access. This commit sums all off-grid hills to get the bias energy to compute the bias factor for WT.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `8cc2ec10f91d`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.