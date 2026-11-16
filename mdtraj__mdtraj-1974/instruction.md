# Issue

**Title:** Test linux aarch64

## Description

Adding linux-aarch64 to the testing matrix. Spin off of #1973. Merge that first.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `3a49c2b8a1a7`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.