# Issue

**Title:** main_nep refactor 2

## Description

### Summary

Decouple model construction and ownership from `Fitness` to prepare for independent NEP model modules.

### Changes

- Centralize model selection in `create_potential()` and move ownership to `main`.
- Move neighbor caches into concrete models.
- Separate construction from one-time initialization while preserving GPU initialization timing.
- Add explicit `find_force()` overrides.

### Validation

- All 23 NEP regression cases passed.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `e3bcdcc10561`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.