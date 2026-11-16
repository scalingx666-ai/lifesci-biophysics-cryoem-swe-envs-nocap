# Issue

**Title:** fix: the macros of determining the device code should be reversed

## Description

This is to ensure that the CPU version of the math functions in `cvm` are compiled correctly.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `a30dfb7bb806`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.