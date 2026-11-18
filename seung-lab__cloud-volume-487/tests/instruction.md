# Issue

**Title:** fix: make parallel image IO more robust

## Description

- Consolidates progress bar
- Checks for errors at end
- Fixes segfault on MacOS
- Accepts iterators
- Fixes file lock synchronization issue on MacOS

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `9bcb69314d61`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.