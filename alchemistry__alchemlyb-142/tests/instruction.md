# Issue

**Title:** Added initial GHA CI

## Description

Executes tests on `ubuntu-latest` with Python versions:
- 3.7
- 3.8
- 3.9

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `40517fd64a5a`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.