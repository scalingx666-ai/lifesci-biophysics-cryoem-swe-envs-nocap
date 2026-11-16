# Issue

**Title:** Extension to pdb format for B-factor and occupancy columns

## Description

This extension include support for read and write the occupancy and B-factor columns from PDB files. The values are stored in the extra dictionary as "occupancy" and "bfactor" respectively.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `b36513d162f9`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.