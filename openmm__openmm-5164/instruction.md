# Issue

**Title:** Prevent running out of disk space in OpenCL CI build

## Description

The CPU OpenCL build has started running out of disk space.  This attempts to fix it.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `adfd84c27330`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.