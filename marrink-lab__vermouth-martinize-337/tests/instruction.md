# Issue

**Title:** Fix deploy script

## Description

fixes deploy job to wait until build, lint and docs have passed.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `6e4c68fd1f51`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.