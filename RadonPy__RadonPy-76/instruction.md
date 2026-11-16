# Issue

**Title:** Release RadonPy 0.2.11

## Description

Bug fix: Attribute error due to removal of the etkdg.maxAttempts attribute from RDKit.
Bug fix: Error reading of LAMMPS log file in LAMMPS 22Jul2025.
Bug fix: Attribute error in sim.lammps.MolToLAMMPSdataBlock.

This release may be the final version in the RadonPy 0.2.x series.
Development of RadonPy will now progress to the next phase towards RadonPy 1.0.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `956877f1c1be`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.