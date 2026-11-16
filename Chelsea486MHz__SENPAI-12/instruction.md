# Issue

**Title:** Some fixes and improvements, new potential optimisation

## Description

Nothing much to see. The main improvement is the switch from pure gradient descent to a two-stage potential reduction algorithm.

The first stage is to perform random transformations on the system and discard the transformations that increase the potential. The second stage is the classic gradient descent.

So yeah, huge pre-simulation speedups.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `c702be312b01`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.