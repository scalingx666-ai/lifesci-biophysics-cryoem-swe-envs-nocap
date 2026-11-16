# Issue

**Title:** Extension to orcalog format to read in enegies from the scf cycle

## Description

A small extension which stores the energies of the scf cycle in the extra dictionary as "scf_energies" of the IOData class.
(It will only keep the energies of the last cycle)

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `b36513d162f9`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.