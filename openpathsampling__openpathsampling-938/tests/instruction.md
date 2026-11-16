# Issue

**Title:** OpenMM instantaneous temp should work if positions not set

## Description

Currently, for an OpenMM snapshot, `snapshot.instantaneous_temperature` will raise `Exception: Particle positions have not been set` if no snapshot has been loaded into the engine (e.g., via setting `engine.current_snapshot`). Analysis will frequently not set that, and so it should be possible to calculate the instantaneous temperature without having done that.

This updates the tests for this and other OpenMM engine-based features to ensure that they work if the engine hasn't set its snapshot yet.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `7c7cf7cc864d`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.