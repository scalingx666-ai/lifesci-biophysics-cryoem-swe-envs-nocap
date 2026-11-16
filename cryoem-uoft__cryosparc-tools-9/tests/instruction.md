# Issue

**Title:** fix: correct MASK_IDX bitshift in C dataset

## Description

Without this we are limited to ~32k datasets in memory. Includes test to catch similar issues

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `938754632e4e`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.