# Issue

**Title:** main_nep refactor 11

## Description

### Summary

Clean up NEP internal interfaces and avoid unnecessary training allocations during prediction.

### Changes

- Keep model-header parsing internal and remove unused declarations.
- Centralize parameter layout conversions for model and restart I/O.
- Initialize loss workspaces only in training mode

### Validation

24 NEP regression tests passed.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `31ee2c866301`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.