# Issue

**Title:** fix ignore and missing test files

## Description

### Summary

Fix missing regression test inputs that were unintentionally ignored by Git.

### Changes

- Change `core` to `/core` in `.gitignore`.
- Add six missing input files under `tests_regression/cases/core/`.

### Validation

- Regression tests: 195 passed, 0 failed.
- Relations tests: 23 passed, 0 failed, 0 skipped.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `5d3724efd9a0`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.