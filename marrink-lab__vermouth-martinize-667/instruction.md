# Issue

**Title:** correct logic for writing out contact map to an external file

## Description

per #666 the logic wasn't correct for writing out the contact map calculated using `-go`. This PR fixes it

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `2c06dbf105a1`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.