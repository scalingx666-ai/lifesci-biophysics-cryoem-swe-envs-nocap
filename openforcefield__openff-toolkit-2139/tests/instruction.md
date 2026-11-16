# Issue

**Title:** Fix conda CI

## Description

- [x] Add nomkl to rdkit conda CI (same solution as #2137)
- [x] Verify fix works

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `8f25ca2f8e12`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.