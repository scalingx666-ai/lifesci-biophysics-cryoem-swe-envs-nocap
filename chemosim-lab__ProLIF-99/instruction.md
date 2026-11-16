# Issue

**Title:** Fix PDBQT supplier

## Description

Keeps hydrogens from the input PDBQT file instead of replacing them with RDKit-generated ones.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `aded38f98207`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.