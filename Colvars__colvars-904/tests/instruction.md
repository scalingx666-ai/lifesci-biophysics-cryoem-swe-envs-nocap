# Issue

**Title:** Fix: Implement numerically safe switching function

## Description

replacing function and derivative with order 1 Taylor expansions around indeterminate form 0/0 for original expression

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `c29f553ed83b`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.