# Issue

**Title:** Raise Exception when no file has been matched in ABFE workflow

## Description

Related to  I noticed that when no file has been matched by the directory, prefix and suffix, no useful error message is being displayed. This PR should fix this issue.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `8e9c31790280`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.