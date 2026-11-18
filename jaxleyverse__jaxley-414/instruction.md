# Issue

**Title:** Add epsilon to branch_len

## Description

This avoids issues with compartments (e.g. somas) that are traced with a single point.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `2501c86db693`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.