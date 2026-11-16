# Issue

**Title:** Volume for conductivity calculation

## Description

Implementation of summation of mean volume for conductivity calculation for dtype identical. This is needed to get the correct ion density when dtype identical is used, since the number of particles of each parallel trajectory is summed up. Using the mean volume is needed for NPT trajectories.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `26466056fe35`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.