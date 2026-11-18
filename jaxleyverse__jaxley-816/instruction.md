# Issue

**Title:** Fix regression CI

## Description

Old regression tests were hard to use and not really convincing. Switched to local only workflow now, comparing HEAD against other git ref such as main.

PR was mostly authored by Claude.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `fbf38a5cb8cd`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.