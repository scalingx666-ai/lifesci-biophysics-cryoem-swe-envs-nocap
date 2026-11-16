# Issue

**Title:** XFEL GUI: support psana2 in joint refinement

## Description

Psana2 uses MPI ranks 0 and 1, so this PR adds a new parameter, mp.psana2_mode=False, that when True, accounts for those ranks not being available.  This is only relevant for mpi_integrate, which is used during time dependent ensemble refinement.

The PR splits the MPI communicator in two, isolating ranks 0 and 1 during the input and balance steps, then brings them back for the integrate step.  Since psana2 needs to start on all ranks, the imageset path is broadcast to all ranks and loaded, but only used on ranks 2+.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `a77a654cb37e`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.