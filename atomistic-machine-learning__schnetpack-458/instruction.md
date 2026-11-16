# Issue

**Title:** Cached NBL takes nbh_transforms

## Description

applying nbh_transforms before caching speeds up the training (in case of FilterNeighbors roughly by factor 3)

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `0b77023869de`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.