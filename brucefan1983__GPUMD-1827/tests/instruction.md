# Issue

**Title:** main_nep refactor 1

## Description

### Summary

Begin the `main_nep` refactor by separating SNES initialization, training, and prediction.

### Changes

- Move execution out of the constructor into `SNES::run()`.
- Extract training and prediction functions, preserving calculations and execution order.
- Add a fixed-model prediction regression test covering multiple batches.
- Remove hard-coded test counts from the PPPM guide.

### Validation

All 23 NEP training and prediction regression cases passed against `src_old/nep`.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `e47d0047fa12`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.