# Issue

**Title:** Always safely overwrite checkpoint and state files

## Description

Addresses the first of two items from   `Simulation.saveCheckpoint()` and `Simulation.saveState()` should now ensure that if OpenMM is interrupted while overwriting an existing file, the existing checkpoint data will not be lost.  Previously, only `CheckpointReporter` did this, and there was a possibility to overwrite another file if one happened to have been manually created with the same name that `CheckpointReporter` would choose for a temporary file.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `bb3073d4f372`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.