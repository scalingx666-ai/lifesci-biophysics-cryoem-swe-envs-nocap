# Issue

**Title:** Rethink swap

## Description

This pull request introduces a significant overhaul to the composition transformation workflow, especially around how atom types are mapped and written to LAMMPS data files, and adds comprehensive tests to verify the correctness of these transformations. The changes ensure that each unique transformation mapping is assigned a unique atom type, improving compatibility with LAMMPS.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `64775413663b`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.