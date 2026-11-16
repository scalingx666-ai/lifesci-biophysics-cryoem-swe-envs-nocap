# Issue

**Title:** MDP files should not be written as binary

## Description

Python3 defaults to `mode=w+b`, so this change was probably missed when updating from 2to3.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `a03da967ce7a`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.