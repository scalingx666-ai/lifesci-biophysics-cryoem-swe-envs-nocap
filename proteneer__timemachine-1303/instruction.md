# Issue

**Title:** Sample velocities when setting up initial states

## Description

* Adds a seed to avoid using global RNG
* We have talked about this, seems like a reasonable improvement over zeros

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `884f05814ad4`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.