# Issue

**Title:** Change all np.NaN to np.nan

## Description

This pull request includes a minor update to the `gather_results` function in `calphy/postprocessing.py`. The change replaces the use of `np.NaN` with `np.nan` for consistency in representing missing free energy values.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `602080e1a4f8`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.