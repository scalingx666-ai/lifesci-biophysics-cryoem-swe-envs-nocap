# Issue

**Title:** Fix neighbor extension compilation looking for non-existent backwards.cu

## Description

The neighbor extension compilation looks for a file that does not exist anymore.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `b7b902f6ea7d`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.