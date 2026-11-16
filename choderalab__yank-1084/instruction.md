# Issue

**Title:** MPI exceptions logging and more robust NetCDF closures

## Description

I've made a few small modifications to fix the stack trace that is logged when an exception is raised in an MPI process and to make more robust the NetCDF handling. In particular,

- I've implemented `AlchemicalPhase.__del__` and make `yank.experiments.Experiment` force garbage collection after deleting the alchemical phase.
- I've minimized the number of `Dataset.open/close()` in the multi-state samplers.
- The reporter now attempts to open the NetCDF file 5 times before giving up and raising the error.

The `switch_phase_interval` is working much better for me after these modifications, which is hopeful.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `ff89e7f7dc58`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.