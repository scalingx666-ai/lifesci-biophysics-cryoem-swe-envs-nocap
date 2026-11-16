# Issue

**Title:** Fix uninitialized MPI count read for queued jobs

## Description

Queued job preparation can leave `nr_mpi` uninitialized because it is populated while constructing local commands. The final guard evaluated `nr_mpi` before checking `do_queue`, causing queued jobs to read it unnecessarily. Checking `do_queue` first short-circuits that read while preserving the existing MPI warning for local jobs.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `f6d3ad20cde0`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.