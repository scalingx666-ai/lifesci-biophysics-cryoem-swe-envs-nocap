# Issue

**Title:** mpi_abort_on_exception: do nothing if communicator size is 1

## Description

This prevents confusing error messages in some cases where the system has mpi4py but we are running single-process. One example is `cctbx.xfel.merge -h`, which currently prints:

```
Abort(1) on node 0 (rank 0 in comm 0): application called MPI_Abort(MPI_COMM_WORLD, 1) - process 0
```

at the end of its output because it hits a SystemExit inside the wrapped function. With this PR, in the given case we won't attempt to clean up the nonexistent MPI communicator.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `8a43ebad55cd`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.