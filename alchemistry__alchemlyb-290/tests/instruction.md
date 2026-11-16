# Issue

**Title:** Make output directory when it didn't exist for workflow

## Description

In  it is reported that when the output directory didn't exist, the flow will crash. This PR make it that if the output directory didn't exist, it will be created.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `a8937699f09c`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.