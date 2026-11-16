# Issue

**Title:** Add mixture packing to Modeller

## Description

Adds `Modeller.addMixture()` to pack boxes of arbitrary mixtures of solvent molecules.  .  The cell list implementation in `Modeller` is now compiled and should be faster.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `ba0aa236ac4d`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.