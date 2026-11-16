# Issue

**Title:** Fix engine props for OpenMM 8.1

## Description

In OpenMM 8.1, it seems that we must pass `None` for engine properties. The empty dict is no longer treated the same.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `5b178519d7f4`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.