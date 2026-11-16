# Issue

**Title:** Enable more CI triggers

## Description

- [x] Test merges to `main`
- [x] Test each PR
- [x] Test `main` weakly (every Sunday at 0:00)

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `8fd3106b1ac8`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.