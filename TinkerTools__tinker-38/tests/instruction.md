# Issue

**Title:** Placeions

## Description

This adds a simple feature to xyzedit to place ions around a solute molecule.  The user must provide the atom number range for the solute molecule and the program randomly selects solvent molecules to be replaced by the specified ion.  As a rule, no ions are placed within a 5 angstrom radius of the solute. The ions are placed at the center of mass of the solvent molecule that they replace.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `8bb21e56e895`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.