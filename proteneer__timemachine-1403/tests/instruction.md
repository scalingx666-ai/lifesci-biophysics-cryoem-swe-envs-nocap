# Issue

**Title:** Reduce MAX_FORCE_NORM by factor of 5

## Description

* May still be too high, but have seen failures >10k

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `28a08660f16f`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.