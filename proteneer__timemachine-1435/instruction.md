# Issue

**Title:** Reduce Final Docker Image Size

## Description

* Only need curand and cudart, can avoid hundreds of MBs of data by copying curand from the devel image
* Reduced container size from 5.87GB to 3.18GB, by removing /opt/nvidia and only keeping cudart/curand.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `e7e9223e7095`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.