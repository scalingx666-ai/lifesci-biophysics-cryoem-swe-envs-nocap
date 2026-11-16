# Issue

**Title:** Fix bug for angle restraints

## Description

The OpenMM interface is incorrect when there are multiple angle restraints

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `c4169e99fc06`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.