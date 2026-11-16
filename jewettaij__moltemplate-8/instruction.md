# Issue

**Title:** Cooke model changes & emcprm2lt fix

## Description

Changed cooke model, was double counting the repulsions, accounted for in tabulated_potential.dat and in fene bond style for LAMMPS. Also provided fix for emc nonbond param, wrong equiv were printed.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `5f15776891f1`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.