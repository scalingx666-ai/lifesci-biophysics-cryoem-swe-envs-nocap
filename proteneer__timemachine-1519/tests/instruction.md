# Issue

**Title:** Refactor SMC

## Description

Merge smc and adaptive smc to make it more flexible.
- Added find_next_lambda function to allow for a custom mechanism to pick the next lambda

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `be95036eb8e5`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.