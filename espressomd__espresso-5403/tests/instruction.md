# Issue

**Title:** Benchmark improvements

## Description

Description of changes:
* allow for tuning and measuring benchmakrs in separate runs, to get tuning out of perf/caliper profiles
* allow to set skin via commandline for lj and p3m
* allow to specify mesh or mesh tuning limits via cmd line for p3m
* Make setting the particle count more intuitive: either --num_particles for an absolute value or --particles_per_core which counts omp_threads*mpi_ranks
* Number of omp threads is available in the python interface via the cell system state

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `49248d7904cc`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.