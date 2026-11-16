# Issue

**Title:** Minor improvements to optimize_coordinates

## Description

* Minor performance improvements, only look at protein/ligand (1/3 of the atoms in some systems, though don't think this makes much if any difference)
* Clean up some type hints
* Clean up docstrings
* Adds a test for the failure

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `b230ca772d9d`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.