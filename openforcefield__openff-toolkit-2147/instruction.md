# Issue

**Title:** Update README and disable Zenodo link checks in CI

## Description

- [x] Fix #2146 by disabling link tests for Zenodo urls
- [x] Update some outdated information in README
- [x] [Lint](https://docs.openforcefield.org/projects/toolkit/en/stable/users/developing.html#style-guide) codebase

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `4dd0dcd81693`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.