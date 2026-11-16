# Issue

**Title:** main_nep refactor 10

## Description

### Summary

Separate NEP runtime-specialization configuration from compilation and library loading.

### Changes

- Move configuration types, construction, validation, and header generation into `nep_compile_config`.
- Keep compilation, dynamic loading, and kernel dispatch in `NEP_Compile`, preserving existing behavior.

### Validation

24 NEP regression tests pass.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `8553f66209c4`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.