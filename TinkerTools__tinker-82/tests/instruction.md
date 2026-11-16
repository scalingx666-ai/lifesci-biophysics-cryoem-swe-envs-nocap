# Issue

**Title:** Add Tinker-OpenMM CMake module and improve CMake build instructions

## Description

- CMake minimum set to 3.9 to find/link OpenMP properly
- Version is now extracted from the promo.f file to update automatically
- By setting the RPATH variables, the user does not have to set `LD_LIBRARY_PATH` to link to the proper libraries
- CMake module for Tinker-OpenMM allows easy building and linking to OpenMM libraries

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `e4d41f486a3d`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.