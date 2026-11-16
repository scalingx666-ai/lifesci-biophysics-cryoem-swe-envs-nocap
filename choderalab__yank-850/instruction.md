# Issue

**Title:** Fix #808: Parallel MPI nodes try to setup the same molecule.

## Description

The bug was caused by the non-deterministic order of `dict`s when assigning the parallel nodes to molecules/systems to set up.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `ece8cb8584b0`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.