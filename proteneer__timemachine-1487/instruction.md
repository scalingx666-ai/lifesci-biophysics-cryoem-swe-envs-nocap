# Issue

**Title:** Parallelize tests

## Description

- Adds `pytest-xdist` to test dependencies
- Adds `-n auto` to pytest args in CI. This runs a number of test processes equal to the number of available cores

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `152f3ab94237`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.