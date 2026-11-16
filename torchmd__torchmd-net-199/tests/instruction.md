# Issue

**Title:** Fix bug in TN

## Description

A dX**2 operation should be a torch.matmul(dX,dX).

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `a11684710d6b`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.