# Issue

**Title:** main_nep refactor 8

## Description

### Summary

Clarify data preparation and `Fitness` initialization while preserving training and prediction behavior.

### Changes

- Move batch reordering from `read_structures()` into `Fitness`.
- Extract dataset preparation, size calculation, workspace initialization, and loss-file setup into private methods.

### Validation

24 NEP regression tests passed.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `57d8f1af8f9c`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.