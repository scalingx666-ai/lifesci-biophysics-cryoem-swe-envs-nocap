# Issue

**Title:** Add a seed keyword to nep.in for the SNES random draws

## Description

This PR adds a `seed <n>` keyword to `nep.in`. It seeds both random generators of SNES. The `std::mt19937` in `SNES::initialize_rng` draws the initial mu. The curand states set up in `initialize_curand_states` draw the population of every generation. Without the keyword, the initial mu is seeded from the clock (12345678 under `-DDEBUG`) and the population draws use the fixed seed 1234567, as on master. The report line reads `(input)   random seed = <n>.` or `(default) random seed not set.`

This PR makes two runs of one input draw the same random numbers, which a benchmark and a test that compares two runs both need. Two seeded runs of the test input write bit-identical `loss.out` and `nep.txt`. The float `atomicAdd` in the force kernels still separates long runs. On a 320-atom set, three seeded runs diverge between generation 1900 and 2100. `doc/nep/input_parameters/seed.rst` states this limit.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `6d10c5efc892`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.