# Issue

**Title:** adopt SPEC 0 instead of NEP 29

## Description

- fix #369
- once this is fixed, we can update the JOSS paper draft PR #328 to only mention SPEC 0

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `6f403f423b15`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.