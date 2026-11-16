# Issue

**Title:** xfel.merge: fix reindex_to_reference

## Description

reindex_to_reference is the less commonly used alternative to modify_cosym

- Correctly handle merge_anomalous
- Handle None properly

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `3af35002d7cd`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.