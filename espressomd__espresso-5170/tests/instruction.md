# Issue

**Title:** Batch particle slice properties setter

## Description

Until now setting properties on the `ParticleSlice `class made multiple expensive calls **per particle in the slice** into the script interface to set each particle's value iteratively. Now a batch update is performed, similar to .

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `06d9d280c089`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.