# Issue

**Title:** Bug fix for building 1-4 exclusion list for GROMACS

## Description

1-4 exclusion should be built from bonds, instead of dihedrals. In some cases, dihedrals are ignored intentionally.
Minor fix for the format of top and pdb file.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `3db8d2ce3291`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.