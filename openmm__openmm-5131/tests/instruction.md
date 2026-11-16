# Issue

**Title:** Refactor reference PME

## Description

The reference PME implementation was originally adapted from Gromacs, and it was still mostly written in C.  This modernizes it and converts it to C++.  ReferencePME is now a class, memory management is done automatically, there are no more mallocs, etc.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `c24c619e4f1f`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.