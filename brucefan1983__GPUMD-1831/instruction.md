# Issue

**Title:** main_nep refactor 5

## Description

### Summary

Decouple prediction from SNES.

### Changes

- Move prediction model loading into `nep_io`.
- Run prediction directly from `main`, constructing SNES only for training.

### Validation

24 regression tests passed.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `b4dcae930581`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.