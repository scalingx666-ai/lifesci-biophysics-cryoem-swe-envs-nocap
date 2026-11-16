# Issue

**Title:** fix: prevent invalid external job input/output group names

## Description

Only allow alphanumeric characters and underscores for custom input/output names. This ensures that names with slashes or other special characters do not break the path when saving external results.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `688c705ce042`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.