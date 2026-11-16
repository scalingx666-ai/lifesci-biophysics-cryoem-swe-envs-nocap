# Issue

**Title:** Add formal charge to the `to_dataframe()` and `from_dataframe()` methods

## Description

This PR is to addressing issue #2001 
1. Atoms with formal charges are now supported by `to_dataframe()` and `from_dataframe()`
2. Unit tests for these two functions are provided

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `39ee1a883a03`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.