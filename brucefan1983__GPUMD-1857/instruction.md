# Issue

**Title:** move nep_evaluate

## Description

### Summary

Move NEP evaluation code into the common module for reuse by other training models, without changing behavior.

### Changes

- Move `nep_evaluate.cu` and `.cuh` from `main_nep/nep/` to `main_nep/common/`.
- Move the `NEP_Compiled_Evaluation` constructor into `nep_evaluate.cu` and update the include path.

### Validation

All 24 NEP regression cases passed against the pre-PR baseline: 23 training cases and 1 prediction case (`--suite full --program nep`).

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `c7e6340a0d1a`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.