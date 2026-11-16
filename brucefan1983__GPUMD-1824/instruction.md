# Issue

**Title:** Extend WPE stage API with qNEP PPPM spacing

## Description

### Summary

Extend the WPE stage API to support configurable qNEP PPPM mesh spacing.

### Changes

- Append `qnep_pppm_spacing` to `WpeStageInfo`.

### Validation

- Verified that only the public WPE API header was modified.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `9bbaed439334`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.