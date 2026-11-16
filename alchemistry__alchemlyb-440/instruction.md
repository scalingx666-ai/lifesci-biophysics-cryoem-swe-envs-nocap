# Issue

**Title:** Add Python 3.14 support and update CI/CD workflows

## Description

- Add Python 3.14 to supported versions in pyproject.toml
- Update GitHub Actions workflows to test Python 3.14
- allow numpy >= 2.3.3
- Update CI matrix to exclude Python 3.12 on macOS/Windows for efficiency
- Upgrade GitHub Actions to latest versions (checkout@v5, setup-python@v6)
- Switch from conda to pip for deploy workflow dependencies
- Update pytest to use auto parallelization instead of fixed cores
- update CHANGES

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `51e26bf3cc4e`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.