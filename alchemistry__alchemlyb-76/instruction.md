# Issue

**Title:** make NAMD parser return energies in kT

## Description

fix #75 

- move NAMD FEP test from parsing to test_fep
- take T as argument

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `38b33bcdaee1`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.