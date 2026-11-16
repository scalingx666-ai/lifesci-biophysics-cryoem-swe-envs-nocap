# Issue

**Title:** Remove versioneer

## Description

Versioneer does not officially support Python 3.12, and we have not been using it.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `82251f564b72`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.