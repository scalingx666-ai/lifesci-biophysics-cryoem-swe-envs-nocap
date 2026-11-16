# Issue

**Title:** Lammps PBC fix

## Description

Use periodic shifts instead of cell-relative shifts for periodic boundary conditions, as expected by SchNetPack.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `662ea1cc96ba`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.