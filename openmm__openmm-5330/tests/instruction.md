# Issue

**Title:** Fix Windows CI

## Description

CI for Windows has been broken since the , so this updates the path to `vcvarsall.bat`.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `9ff6e6945542`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.