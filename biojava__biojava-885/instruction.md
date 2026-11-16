# Issue

**Title:** Upgrade to latest ciftools

## Description

- upgrade ciftools version (dedicated release that supports Java 8, properly supports arbitrary schemata)
- removing the old mmCIF parser requires some discussion and is probably better of in a separate PR

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `b9519e4c568e`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.