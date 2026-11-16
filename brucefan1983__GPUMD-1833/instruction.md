# Issue

**Title:** main_nep refactor 7

## Description

### Summary

Separate NEP initialization and restart handling from the SNES optimizer.

### Changes

- Move restart I/O and parameter layout conversions into `nep_io`.
- Move parameter initialization, fine-tuning, and parameter grouping into `nep_init`.
- Keep GPU transfers and optimization logic in `SNES`, preserving existing file formats and numerical behavior.

### Validation

24 NEP regression tests passed.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `b64a6f021094`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.