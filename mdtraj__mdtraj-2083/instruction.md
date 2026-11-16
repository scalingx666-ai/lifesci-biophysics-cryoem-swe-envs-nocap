# Issue

**Title:** Use repeated bonds in PDB CONECT records to increase bond orders (REMAKE of #1848)

## Description

This is a remake of #1848. . 

Essentially, it allows user to duplicate bonds in the pdb `CONECT` section to represent higher bond orders. This is out of regular PDB spec.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `573a85c22c04`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.