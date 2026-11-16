# Issue

**Title:** Use kokkos 4.7.03 and handle cuda 13

## Description

Required to use the current `module load cudatoolkit` (unversioned) on Perlmutter

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `25b43e184a6d`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.