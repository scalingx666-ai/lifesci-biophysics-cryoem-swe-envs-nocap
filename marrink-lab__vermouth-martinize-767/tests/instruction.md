# Issue

**Title:** memory optimisation of contact map

## Description

reimplementation of #733 with some slight renaming of functions and extra logging. I have not validated how much quicker/memory efficient this is.

- remove some uses of numpy
- rename functions for better descriptions
- add debug logging
- tests passing

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `2a4114d1c616`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.