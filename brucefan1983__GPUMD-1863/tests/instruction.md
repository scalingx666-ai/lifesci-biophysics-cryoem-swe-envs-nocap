# Issue

**Title:** rename qnep2_vdw to qnep3_vdw

## Description

### Summary

Rename `qnep2_vdw` to `qnep3_vdw` without changing numerical behavior.

### Changes

- Update training output, model readers, MD dispatch, converter, and documentation.
- Remove the old name without retaining an alias.
- Preserve existing `qnep1` and `qnep2` behavior.

### Validation

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `dc07d0f0678f`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.