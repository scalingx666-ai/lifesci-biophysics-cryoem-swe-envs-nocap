# Issue

**Title:** gmx parser (_extract_dataframe) using pd.read_csv for performance

## Description

pd.read_csv() shows the same behavior building pandas Dataframe with better performance. Parsing header lines remain the same but with `bytes` reading, instead of `text` mode.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `486998f10d27`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.