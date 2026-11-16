# Issue

**Title:** main_nep  refactor 13

## Description

### Summary

Refactor TNEP training code for readability and reuse, keeping dipole and polarizability in one implementation.

### Changes

- Separate initialization, parameter binding, and evaluation stages.
- Reuse scaler and output-clearing helpers from `nep_common`.
- Add error checking for device selection during initialization.
- Preserve numerical algorithms, native/JIT dispatch, and multi-GPU execution order.

### Validation

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `ad995f1ae3f0`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.