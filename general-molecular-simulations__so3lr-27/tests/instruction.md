# Issue

**Title:** NVE CLI fix

## Description

The NVE ensemble crashed before starting, because the variable nhc_thermo was dropped, but still used for nhc_tau calculation. 
I added a simple if statement setting nhc_tau=0 for NVE only and kept its definition for other ensembles.  The NVE simulation ran after this fix. I believe it shouldn't negatively affect the simulation results.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `32129bf0ee84`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.