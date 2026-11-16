# Issue

**Title:** train_mode to model_type internally

## Description

### Summary

Rename `train_mode` to `model_type` in `main_nep` to accurately describe model selection and align with the input keyword.

### Changes

- Rename related variables, parser functions, and the JIT macro consistently.
- Update associated comments and diagnostic messages.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `263d3f10838c`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.