# Issue

**Title:** Keep Residue Ids when writing CIF files

## Description

Keep residue ids from the topology when writing cif files using the CIFWriter. This allows users who use truncated proteins in FEP to keep residue ids consistent when looking at trajectories or representative structures from the simulation.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `50945a5d5140`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.