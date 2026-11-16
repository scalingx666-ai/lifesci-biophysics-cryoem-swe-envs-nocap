# Issue

**Title:** Improve LB particle coupling performance on the CPU

## Description

Description of changes:
- extract the LB CPU particle coupling code and remove redundant copy operations

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `91f278048d33`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.