# Issue

**Title:** Disable beta/rc CI

## Description

Beta/RC CI has been failing for months, this PR removes it.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `4fa2f7c0a2ff`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.