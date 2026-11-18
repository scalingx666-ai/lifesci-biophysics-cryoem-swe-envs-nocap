# Issue

**Title:** perf: increase single voxel access performance for compressed seg

## Description

This is orders of magnitude faster for unsharded segmentation on an SSD. About 10-20% faster for remote segmentation. About 30% faster for sharded on SSD.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `b4e24d7b6c99`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.