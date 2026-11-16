# Issue

**Title:** Optimize coordination with batch distance calculations

## Description

This is an optimization of CoordinationBase that computes distances in batches. Tested on Leonardo with a standard coordination benchmark gives me these speedups on the PLUMED-side part (with plumed benchmark):

Run | Before | Batched | Total speedup
-- | -- | -- | --
1 OpenMP thread | 33.157 s | 25.706 s | 22.5%
2 threads | 16.795 s | 14.213 s | 15.4%
4 threads | 8.908 s | 7.633 s | 14.3%
8 threads | 5.129 s | 4.635 s | 9.6%
2 MPI ranks | 17.985 s | 14.165 s | 21.2%

Improvements are consistent with those measured in my Mac. I still have to test on Actions (running now)

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `fd26c15e7fee`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.