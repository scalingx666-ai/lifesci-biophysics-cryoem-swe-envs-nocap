# Issue

**Title:** Implemented "count in voxel" function

## Description

This analysis command is meant to be used with GIST analysis to estimate water residence time for a given region. 

When running GIST on a trajectory, we get a list of voxels and their associated rotational/translational entropy of solvent atoms. We can focus on low-entropy voxels (which behave differently from the bulk) and find the population of solvent atoms in a voxel throughout the course of a simulation to measure how long it's taking for solvent molecules to equilibrate in and out of solvent pockets on the macromolecular interface (survival time correlation function).

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `07d176152af9`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.