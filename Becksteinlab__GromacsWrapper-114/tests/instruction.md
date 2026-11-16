# Issue

**Title:** Small fix for recent changes in numpy.

## Description

Can't index with floats anymore, so we use ints here.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `9fe10ee3909e`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.