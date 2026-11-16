# Issue

**Title:** use model keyword

## Description

### Summary

Use the `model` keyword to select the different NEP models.

### Changes

- Add the `model` keyword.
- Keywords such as `model_type`, `charge_mode` can still be used, but will be deprecated upon releasing GPUMD-v6.0.

### Validation

NEP-related regression tests and pytests passed.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `46c4b301dd61`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.