# Issue

**Title:** Refactor/use rdkit functions for utils and poly module

## Description

I refactored the two functions in a simple way.

I uses `Descriptors.MolWt` in `polymer_stats.py`, but note that we need to use `Descriptors.HeavyAtomMolWt` if we want the behaviour to be exactly the same as before the change.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `94b52ac73780`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.