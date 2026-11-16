# Issue

**Title:** Disable persistent_workers completely, enable pin_memory in DataModule

## Description

Change the default config of the DataModule.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `d8813f5d23c6`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.