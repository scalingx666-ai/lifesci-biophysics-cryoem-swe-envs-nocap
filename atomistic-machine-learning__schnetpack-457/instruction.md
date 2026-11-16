# Issue

**Title:** Updates for response experiments / NMR utilities

## Description

- Added transform and postprocessor for splitting NMR shielding tensor into different elemental contributions
- Added custom MAE metric for diagonal / offdiagonal tensor elements
- Added config for basic experiment with response properties (including dipoles, polarizabilities, forces and NMR shielding)

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `f1088a3806f2`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.