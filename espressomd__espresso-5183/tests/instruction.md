# Issue

**Title:** parallel particle slice getter

## Description

Until now getting particle properties from `ParticleSlice` objects was very expensive since multiple expensive MPI calls **for every particle in the slice** were made.

- Moved parts of the implementation of ParticleHandle's get_attribute method to the script interface.

- Implemented optimized getter for positions, types and charges and general getter for all other attributes except special attributes that the python class still handles.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `a1d0c994e533`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.