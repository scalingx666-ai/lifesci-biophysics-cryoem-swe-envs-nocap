# Issue

**Title:** Ensure a constant batch size when computing flow fields to avoid excessive jitting.

## Description

Ensure a constant batch size when computing flow fields to avoid excessive jitting.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `52a730b70fc4`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.