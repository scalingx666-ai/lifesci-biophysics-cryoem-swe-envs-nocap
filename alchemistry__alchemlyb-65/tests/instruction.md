# Issue

**Title:** Set Travis CI to test against Python 3.7

## Description

Requires us to use `dist: xenial` (Ubuntu 16.04) to add this Python version to the test matrix.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `d7d2663d2222`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.