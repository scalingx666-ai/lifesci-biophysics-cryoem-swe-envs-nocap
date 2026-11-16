# Issue

**Title:** CIF parsing: entity type should be case insensitive

## Description

Some PDB-IHM files use upper case, unlike PDB files that use lower case.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `602b4d52b0fe`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.