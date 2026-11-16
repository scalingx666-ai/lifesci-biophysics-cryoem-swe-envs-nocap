# Issue

**Title:** added CUDA kernel for random_velocities!

## Description

I have added a CUDA kernel for random_velocities!()

Performance improvement has been measured for a dense LJ system with fixed average density that has been simulated with the  Langevin algorithm for varying number of particles and box sizes. Benchmarked on a NVIDIA GeForce RTX 4090 I get:

before changes: commit 0f19f896
simulation 0 took 1.884167053 for 100 atoms
simulation 2 took 1.780424146 for 400 atoms
simulation 4 took 2.333971938 for 1600 atoms
simulation 6 took 3.446783882 for 6400 atoms
simulation 8 took 8.438397874 for 25600 atoms
simulation 10 took 31.827900505 for 102400 atoms


random_velocities!: commit 92eba91d
simulation 0 took 1.235772652 for 100 atoms
simulation 2 took 1.212126355 for 400 atoms
simulation 4 took 1.446382735 for 1600 atoms
simulation 6 took 2.0191413 for 6400 atoms
simulation 8 took 3.58989918 for 25600 atoms
simulation 10 took 11.58562941 for 102400 atoms

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `c069e747b5e8`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.