# Issue

**Title:** Conda install

## Description

This pull request implements assignment of RNA backbone suites in LOOS
* C++ class in src/RnaSuite.* for assigning backbone suites from an AtomicGroup
* Definition of reference suites in $LOOS/share/suitename_definitions.dat; install handled by share/SConscript
* SWIG wrapper to allow suite assignment in pyloos
* Tool in Tools/rna_suites.cpp for assigning suites to a selection for each frame in a trajectory

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `738ab192d9d2`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.