# Issue

**Title:** Speed up energy decomposition

## Description

Noticed that it could take awhile for simulations to 'finish' after HREX had finished and upon investigation it looks like we are spending a lot of time reading frames from disk. And due the way the loop was constructed for energy composition we were reading the frames into memory repeatedly.

This only makes a noticeable difference for HREX ( <=~1% for bisection/sequential), which is because there are many more disk reads due to only writing a single frame to disk in the StoredArrays per chunk, unlike sequential/bisection which writes chunks of 100 frames to disk. We probably want to resolve that in a subsequent PR.

# Benchmarks

Cuda Arch 8.6
Nvidia A10

## Vacuum

HREX now gets faster with the more windows :rofl:, shows about a 20% speed up

![image](https://github.com/proteneer/timemachine/assets/5840832/1552c00a-1b2c-4011-8341-d566ac45dad6)

### Speed up relative to master

![image](https://github.com/proteneer/timemachine/assets/5840832/15a5ed52-fe3b-42c9-99b2-617c261b4616)

## Solvent

![image](https://github.com/proteneer/timemachine/assets/5840832/1ffd6428-aa8c-47f1-a350-c3cfda9b563f)

### Speed up relative to master
Up to 5%

![image](https://github.com/proteneer/timemachine/assets/5840832/e0c2afda-813b-42b9-985a-3454d68f0d98)

## Complex

![image](https://github.com/proteneer/timemachine/assets/5840832/17f48104-77c3-43d7-9a03-d0b3fa6009db)

### Speed up relative to master
![image](https://github.com/proteneer/timemachine/assets/5840832/58ab032f-fa26-4ff6-bba2-d02c42cc062b)


- [x] Verify that the performance difference between HREX and others is the chunking behavior

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `87cebc96f469`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.