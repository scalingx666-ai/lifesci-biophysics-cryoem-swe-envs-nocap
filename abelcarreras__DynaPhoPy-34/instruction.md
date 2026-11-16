# Issue

**Title:** Cleanup deprecated and removed NumPy APIs in C extension

## Description

NumPy recently was upgraded to 2.0, and a lot of old deprecated APIs were removed. This PR addresses these changes.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `592efebab227`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.