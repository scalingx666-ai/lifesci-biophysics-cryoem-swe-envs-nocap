# Issue

**Title:** Moves OpenMM imports to function calls

## Description

At some point we should more fully isolate OpenMM from core code.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `02d5366ffdeb`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.