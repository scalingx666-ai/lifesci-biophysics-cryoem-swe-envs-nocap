# Issue

**Title:** OpenMP patches

## Description

Description of changes:
- properly propagate Kokkos CMake options
- cite the Kokkos, Cabana, heFFTe papers
- cleanup the Sphinx pipeline

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `209e6e7927ff`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.