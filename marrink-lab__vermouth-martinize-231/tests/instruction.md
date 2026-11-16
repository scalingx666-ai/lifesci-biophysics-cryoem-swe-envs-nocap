# Issue

**Title:** Bugfix system sorting in pdb_to_universal

## Description

Sorting was done on the old system object and not on the canonicalized copy object that is actually returned and used everywhere.
I got an error with DSSP before, probably because the system wasn't sorted properly.
This change fixed my issue and my system is now coarse grained properly.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `44bd8fee0d6e`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.