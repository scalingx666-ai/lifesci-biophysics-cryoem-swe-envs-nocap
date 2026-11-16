# Issue

**Title:** Added option for which metric to base learning rate decay on

## Description

This supersedes #27.  It was based on a very old version of the code, so it was easier to create a new PR.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `3d6a064a942a`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.