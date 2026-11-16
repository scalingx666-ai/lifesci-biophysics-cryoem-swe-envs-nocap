# Issue

**Title:** Inclusion of MD22 dataset

## Description

I included the MD22 dataset: https://arxiv.org/abs/2209.14865
This dataset is quite new, with much more complex molecules than MD17. MACE has also benchmarked on it.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `3bfaf2632df4`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.