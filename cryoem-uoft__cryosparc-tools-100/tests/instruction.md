# Issue

**Title:** fix: prevent queueing or killing external jobs

## Description

Raises informative error message before external job is submitted to command server.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `029d68be006a`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.