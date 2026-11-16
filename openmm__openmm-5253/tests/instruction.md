# Issue

**Title:** Merge fixes for 8.5.1

## Description

We're preparing to do an 8.5.1 patch release.  This merges the following changes from the main branch: #5236, #5242, #5243, #5251.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `b55e60882dff`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.