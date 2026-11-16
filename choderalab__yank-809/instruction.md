# Issue

**Title:** MPI exception handling

## Description

Fix #798 : NaN handling fails with MPI.

This adds exception handling support in the `mpi.py` module. Briefly, if a node's task raises an exception, it gets propagated the other nodes according to the `propagate_exceptions_to` argument that can assume the values `'all'`, `'group'` or `None`.

I've also added the `_MpiProcessingUnit` context manager. This will probably be the first step toward a more object-oriented API that separates the computational resources from the task execution.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `588b23218d83`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.