# Issue

**Title:** Remove ambermini and redistribution of the states in trailblaze

## Description

I'm testing switching from `ambermini` to `ambertools` here, and I've added an option to redistribute the states after trailblazing based on the std of the potential difference computed in both directions.

I'll have to merge and release choderalab/openmmtools#444 before merging for tests to pass, but they do pass locally.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `251d2719c049`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.