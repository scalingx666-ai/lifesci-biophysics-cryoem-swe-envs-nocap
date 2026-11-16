# Issue

**Title:** Implement automatic determination of processes_per_experiments

## Description

By default, `processes_per_experiment` was set to `None` (i.e. no MPI communicator splitting). This causes problems with SAMS simulations as we support only 1 replica through the YAML interface right now.

This adds the `processes_per_experiment: auto` (now the default) that tries to split the MPI communicator to run as many experiments in parallel as possible given the allocated resources.

I've also started to add in the `mpi` module code to simulate an MPI environment for tests.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `c835d50202ee`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.