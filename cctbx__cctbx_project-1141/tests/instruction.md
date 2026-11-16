# Issue

**Title:** New merging worker prepare_spread

## Description

Quasi-monochromatic SpReAD refinements via the moving-window approach. This PR adds a cctbx.xfel.merge worker that prepares a large Slurm array job containing all the partial-dataset merges and finishing with a parallel run of all Phenix refinements. Equal-width and equal-count binning modes are supported. Some documentation is included.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `8ea042fd9049`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.