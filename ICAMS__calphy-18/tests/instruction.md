# Issue

**Title:** Update/alchemy

## Description

Update alchemy routine to use the hybrid/scale method. This makes it possible to use alchemy and upsampling for any interatomic potential in LAMMPS.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `4be4450dad1c`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.