# Issue

**Title:** Add debug code for the Jacobi failure

## Description

Sometimes the Jacobi procedure just fails and we don't know why. We need some debug code to output the overlapping matrix and correlation matrix when it is failed.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `30d66a38f1de`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.