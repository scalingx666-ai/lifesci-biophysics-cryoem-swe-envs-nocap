# Issue

**Title:** Drop py311 from conda tests

## Description

OpenMMForceFields requires AmberTools <23, but AmberTools <=22 doesn't support python 3.11

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `f282282c6c5f`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.