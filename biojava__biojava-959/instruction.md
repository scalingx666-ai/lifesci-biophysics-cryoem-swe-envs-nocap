# Issue

**Title:** Alignment matrix tests

## Description

- tests for substitution matrices
- javadoc and some reformatting
- also removed some tests that required java 11 and broke Java 8 tests, from a previous PR
-

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `3929dd70910c`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.