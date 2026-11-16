# Issue

**Title:** Clarify component-wise force metrics in parity plot

## Description

The parity plot flattens force arrays before calculating MAE, RMSE, and MaxAE, so these are component-wise metrics rather than per-atom vector-magnitude errors. Label the console section explicitly to make reported benchmark conventions unambiguous. No numerical behavior changes.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `27d9d2182da9`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.