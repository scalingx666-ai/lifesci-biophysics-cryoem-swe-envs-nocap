# Issue

**Title:** Change the DSSP processor to run on Windows.

## Description

In addition, change some tests to be friendlier to systems with iffy PATH handling

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `d1f0f1997867`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.