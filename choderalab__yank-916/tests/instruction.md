# Issue

**Title:** Add YAML syntax for MCMC moves

## Description

Fix #898 .

For the existing global options that were injected into the MCMC move, I've kept `timestep` and `nsteps_per_iterations` (now `default_timestep` and `default_nsteps_per_iterations` respectively) because I thought these are options we'll have to change often, and I've deleted `collision_rate`, `mc_sigma_displacement` and `integrator_splitting` that I don't remember I ever had to change for production simulations. They can still be set in the `mcmc_moves` block, of course, but let me know if you think I should do things differently.

This requires choderalab/openmmtools#336 (and a new `openmmtools` release) for tests to pass.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `e4b1de6cf310`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.