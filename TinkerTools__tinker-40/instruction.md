# Issue

**Title:** improvements in Tinker-OpenMM interface & shakeup

## Description

1. In Tinker-OpenMM interface, the periodic boundary conditions
are enabled for distance and group restraints.

2. Add if checks for some global "allocatables" in shakeup routine,
letting mechanic routine can be called multiple times in a program.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `55125bbd0261`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.