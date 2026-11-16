# Issue

**Title:** main_nep refactor 4

## Description

### Summary

Refactor NEP model-file handling and fix initialization issues.

### Changes

- Extract model-file checks and the shared element table.
- Validate `type` before use.
- Ignore restart files during prediction and stale model files during fresh training.
- Update the existing pytest and remove an unnecessary comment.

### Validation

All 24 NEP training and prediction regression cases passed.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `7d0187e04655`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.