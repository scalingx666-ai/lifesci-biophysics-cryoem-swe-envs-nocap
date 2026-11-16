# Issue

**Title:** main_nep refactor 6

## Description

### Summary

Separate model file I/O from fitness evaluation and parameter initialization.

### Changes

- Move `nep.txt` writing from `Fitness` and scaler loading from `Parameters` into `nep_io`.
- Keep GPU transfers in `Fitness` and `Parameters`, preserving the existing file format and numerical behavior.
- Rename `Fitness::write_nep_txt` to `write_potential` to distinguish it from the file-format writer.

### Validation

24 regression tests passed.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `1474b520198b`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.