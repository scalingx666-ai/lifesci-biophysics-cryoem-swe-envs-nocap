# Issue

**Title:** Consolidate TMPDIR creation for bash apps

## Description

Every bash app now creates a temporary working directory in the same way.

Also  maybe

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `2b0d72d66532`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.