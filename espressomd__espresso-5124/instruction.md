# Issue

**Title:** Batch particle properties update

## Description

Until now the `update()` method of the `ParticleHandle` class just sequentially set its parameters to new values, making one expensive update call for each new parameter. Now a batch update is performed.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `e414699ff00d`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.