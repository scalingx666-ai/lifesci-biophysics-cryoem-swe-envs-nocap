# Issue

**Title:** main_nep refactor 3

## Description

### Summary

Separate loss computation from SNES by consolidating it in `Fitness`.

### Changes

- Move loss components, NEP4 regularization, and associated workspaces into `Fitness`.
- Let SNES consume total fitness for sorting and updates.
- Move the training header into `Fitness`, preserving loss reporting and calculation order.

### Validation

- All 24 NEP training and prediction regression cases passed.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `70b3a7acff56`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.