# Issue

**Title:** Consistently test with Compute-sanitizer

## Description

* Inconsistency added in #1464

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `8c60d836cdbe`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.