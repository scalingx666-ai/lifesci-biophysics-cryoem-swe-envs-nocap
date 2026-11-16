# Issue

**Title:** Update packaging command in CI

## Description

Calling `setup.py` is deprecated; use `build` package instead per recommendation of https://packaging.python.org/en/latest/tutorials/packaging-projects/

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `e8afd792fa90`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.