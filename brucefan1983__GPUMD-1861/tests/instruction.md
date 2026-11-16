# Issue

**Title:** no initial_para

## Description

### Summary

Remove the `initial_para` keyword while preserving its previous default behavior.

### Changes

- Remove keyword parsing, associated code, documentation, and tests.
- Add missing `default` branches to Spin kernel dispatch to resolve compiler warnings.

### Validation

- All 24 NEP cases in `tests_regression` passed.
- NEP and Spin pytest tests passed.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `ecdcf682e882`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.