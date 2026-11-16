# Issue

**Title:** Stop uploading coverage reports on nightly CI runs

## Description

We had an issue where codecov would fail to upload reports when we uploaded too many with the same commit hash, this will prevent reports being uploaded when we don't expect anything to change.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `304764a32e03`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.