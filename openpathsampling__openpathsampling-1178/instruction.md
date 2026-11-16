# Issue

**Title:** Fix CI: Remove `pkg_resources`; pandas 3; numpy scalars

## Description

Getting CI back to green.

- [x] Remove dependency on `pkg_resources`, which is no longer shipped by default.
- [x] Remove use of `applymap` (apparently removed in pandas 3)
- [x] Fix issue with numpy only allowing 0-dim arrays to convert to scalar

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `72c3dd72fe8d`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.