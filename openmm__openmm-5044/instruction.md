# Issue

**Title:** Don't allow LJPME when LJ interactions use `CustomNonbondedForce`

## Description

Updates Amber, CHARMM, Desmond, `ForceField`, and GROMACS loaders to raise `ValueError` if LJPME is requested on a system that requires `CustomNonbondedForce` to implement its Lennard-Jones interactions.  See https://github.com/openmm/openmm/discussions/5039#discussioncomment-14077098.  This is usually because of NBFIX, but it can also be due to non-standard mixing rules in Desmond and GROMACS.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `3645e1eeb7ce`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.