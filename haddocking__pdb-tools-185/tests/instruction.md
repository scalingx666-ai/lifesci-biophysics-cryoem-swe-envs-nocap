# Issue

**Title:** [MINOR] Add support for hybrid_36 format - issue #184

## Description

Edited `pdb_reatom`, `pdb_tidy` and `pdb_fromcif` to add an option to enable the hybrid_36 format that will allow to handle/write PDB files with more than 99999 atoms.

Added tests for the news options

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `def5bf9929e8`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.