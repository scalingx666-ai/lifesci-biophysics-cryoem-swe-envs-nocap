# Issue

**Title:** Fix tests and more robust netcdf opening

## Description

- Implement the fix suggested in #1165 to work around the netcdf locking issues.
- Fix a crash during exception handling.
- Fixed some tests that were failing with a different message than the expected one.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `7bfeb40ef063`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.