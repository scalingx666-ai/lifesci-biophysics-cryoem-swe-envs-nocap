# Issue

**Title:** Backport updates to LAMMPS interface files

## Description

This PR merges in some small changes from the LAMMPS repo to the FixColvars implementation, as well as numerous changes from this LAMMPS PR: 

To simplify conflict management, `fix_colvars.{cpp,h}` are now being tracked as patches only, consistent with what is currently done in GROMACS and NAMD.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `2f3bd939cd51`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.