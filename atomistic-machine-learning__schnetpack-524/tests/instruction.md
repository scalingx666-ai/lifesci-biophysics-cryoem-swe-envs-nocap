# Issue

**Title:** data.load_properties can now be set to an empty list

## Description

This enables faster data loading in cases where only the structure is needed, e.g. in generative models.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `73bb31bf233d`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.