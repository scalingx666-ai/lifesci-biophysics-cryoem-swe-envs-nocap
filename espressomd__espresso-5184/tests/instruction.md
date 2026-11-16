# Issue

**Title:** Guard against verlet list overflow

## Description

Description of changes:
- Added new function `setOverflow()` and `hasOverflow()` in `custom_verlet_list` and automatically switch to using link cell when Verlet list overflow is detected
- Added new test function in `cell_system.py`

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `dc87ede3f6c2`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.