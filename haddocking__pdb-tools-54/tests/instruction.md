# Issue

**Title:** Fixed MODEL line bug when model number > 10

## Description

Small bug in `pdb_mkensemble` that placed the model number at the wrong column. Splitting with `pdb_splitmodel` in an ensemble of more than 10 models would yield always 10 models (only reading the last digit).

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `09a354e95d4a`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.