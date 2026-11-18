# Issue

**Title:** fix(windows,paths): non-file windows parsing was broken

## Description

Windows parsing was dropping the bucket name from the path.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `0d64902d50fa`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.