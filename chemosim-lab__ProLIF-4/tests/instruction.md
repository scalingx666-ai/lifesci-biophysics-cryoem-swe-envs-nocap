# Issue

**Title:** More documentation and notebooks

## Description

- Adds more documentation and notebooks
- Changes the `to_dataframe` function to:
  - accept as input an ifp list made of atom indices instead of bitvectors
  - accept a new `dtype` argument (default: None, keep original) to convert the bits to other data types
  - accept a new `drop_empty` argument (default: True) to drop columns that exclusively contain "off" bits
- Expends coverage
- Fixes the documentation not building

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `602e8463fe7c`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.