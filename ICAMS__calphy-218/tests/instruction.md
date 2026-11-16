# Issue

**Title:** fix: melting_temperature crashes with n_iterations > 1

## Description

fix: move integrate_reversible_scaling outside loop in MeltingTemp.run_jobs

When n_iterations > 1, integrate_reversible_scaling was called inside
the for loop before all ts.forward_N.dat files existed, causing
FileNotFoundError. Fixed by moving the call outside the loop.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `89725166ab6c`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.