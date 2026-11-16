# Issue

**Title:** main_nep refactor 12

## Description

### Summary

Refactor ordinary and temperature-dependent NEP training for readability and code reuse.

### Changes

- Separate initialization and parameter binding.
- Extract descriptor, ANN, and force evaluation into `nep_evaluate`.
- Extract scaler, descriptor output, output clearing, and ZBL helpers into `nep_common`.
- Preserve numerical algorithms and multi-GPU execution order.

### Validation

All 24 regression tests passed.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `9c5cc1648a6f`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.