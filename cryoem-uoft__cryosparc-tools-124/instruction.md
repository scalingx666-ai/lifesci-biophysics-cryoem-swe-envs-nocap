# Issue

**Title:** fix: ensure tools can load datasets that are exactly 4kiB

## Description

We found an issue where datasets of this file size fail to load due to a bug in numpy < 2.2. Appears to be related to page size on linux.

See  (released in 2.2, but we have to support older versions)

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `2c359eefb9c4`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.