# Issue

**Title:** Small fix to OpenMM torsion assignment

## Description

Fix OpenMM torsion assignment for amoebabio09 FF (for nucleic acids). The 4-,
5-, and 6-fold torsions were not being added to the OpenMM System, and torsions
with these periodicities were defined for some nucleic acid systems. This commit
makes sure that those torsions are correctly added and adjusts the behavior of
adding torsions such that only torsions with non-zero force constants actually
get added.  This reduces the number of "useless" terms in the resulting OpenMM
System and makes debugging easier.

I also added a protective `#ifdef __INTEL_COMPILER` around the ifort-specific
OpenMP directives so that you don't have to keep changing the source code to
compile with ifort or gfortran.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `bb2506bffcf5`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.