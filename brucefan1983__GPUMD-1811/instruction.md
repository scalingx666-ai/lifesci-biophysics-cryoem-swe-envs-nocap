# Issue

**Title:** update regression tests

## Description

### Summary

Remove obsolete PPPM baseline assumptions that caused false regression failures.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `eb7fbbf17a78`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.