# Issue

**Title:** fix: raise errors from green threads

## Description

Green threads were swallowing exceptions.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `ae0fb2da1c6d`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.