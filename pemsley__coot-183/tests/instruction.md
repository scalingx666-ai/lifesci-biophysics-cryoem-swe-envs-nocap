# Issue

**Title:** Use imol_no to get VDW sphere radii.

## Description

VDW radii are all -ve in Moorhen. This patch uses the imol_no in call to get radii, since this was used to add the restraints.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `747dcb7b4416`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.