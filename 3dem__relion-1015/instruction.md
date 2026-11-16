# Issue

**Title:** Change MPI block size and minor SYCL document update

## Description

This will update default MPI block size only when "-DUSE_MPI_COLLECTIVE" is enabled. This is default for ALTCPU and SYCL. It has no impact when "-DUSE_MPI_COLLECTIVE" is not set.
The default 512MB for MPI collective may increase file descriptor usage and result in error or lower performance at multi-node scale.

I've erroneously committed in "relion-devel" branch without PR and there is closed PR to wrong "master" branch", sorry about that.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `da8ee23200ff`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.