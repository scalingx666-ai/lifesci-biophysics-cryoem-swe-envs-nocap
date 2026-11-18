# Issue

**Title:** unnecessary integer casting for floating  chunk size

## Description

I discovered trying to work with a dataset with a floating point resolution that the draco mesh deduplication wasn't working correctly with graphene sharded.. i tracked the problem to this line, where casting the chunksize to integers caused the chunk boundary position prediction to accumulate error over many boundaries.... leading to incorrect detection of chunk boundaries and therefore improper stitching.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `cf94fc4ac97e`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.