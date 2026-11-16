# Issue

**Title:** Fix pushing docs to readthedocs

## Description

Not sure if there's a way to test this without merging the PR, but this is an attempt to fix the readthedocs integration.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `9e0841b98b6d`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.