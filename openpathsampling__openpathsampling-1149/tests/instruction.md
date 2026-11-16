# Issue

**Title:** Update Python version in test-openmm-rc

## Description

Our script to test OpenMM RCs was pinned to Python 3.9, which we no longer support. Looks like there's another OpenMM RC out, so it is time to update this!!

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `6e0b24fb4bc3`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.