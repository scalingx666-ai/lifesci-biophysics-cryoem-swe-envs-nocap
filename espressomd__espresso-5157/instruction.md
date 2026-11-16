# Issue

**Title:** On a single MPI rank, don't use ghost cells in RegularDecomposition

## Description

When running on a single MPI rank, ghost cells aren't necessary, as the
corresponding physical cell is available on the same MPI rank.

In this case, the physical cells can be directly connected as neighbors and the ghost communication deactivated.

the PR somewhat improves the performance of shared-memory parallelizaiton simulations on a single MPI rank by avoiding the non-parallel ghost communication.

@jngrad do we have a CI job that runs the test on just one mpi rank? Aside from this PR, there are other optimizations in ESPREsSo which are only active on sinlge mpi rank simulations.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `cf2c0eb70f78`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.