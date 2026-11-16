# Issue

**Title:** Rms overlap

## Description

This branch of the LOOS master contains a new Tool for comparing sets of trajectories to sets of other trajectories using their all-to-all RMSDs. It is a modification of multi-rmsds allowing it to work on a pair of sets rather than all frames from all trajectories.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `422bc273c2a6`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.