# Issue

**Title:** CI: Python 3.11 and 3.12 package builds

## Description

- Updated to the latest `pypa/cibuildwheel` action
- Ensure dedicated x64 and ARM builds for macOS
- ARM builds for Linux

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `9bf3f656d9ad`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.