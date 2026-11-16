# Issue

**Title:** Pure python overlap.py

## Description

This is part 1 of 2. 

Purpose: make pure python as close to the speed of cython as possible.

In this code shells, then primitives, then angular momentums are looped over. This code uses np.frompyfunc to define a vectorized single dimensional overlap calculation function. For two primitives interacting all angular momentums and x,y,z are then broadcast to this function. rather than 8 for loops, this uses 6.

In Part 2 - currently in progress, the loops over primitives will also be included in the broadcasting. Rather than 8 for loops this uses 4.

**Test 1: loading big.molden using (old) i7 2.4 GHz desktop**
python + cython: 33.29 seconds,  average over 50 iterations
**Pure python timings:**
No shell or primitive screening: 93.49 seconds,  average over 10 iterations
With shell screening:  52.14 seconds,  average over 10 iterations
With shell and primitive screening: 49.76 seconds,  average over 50 iterations

**Test 2: overlaps on small molecule - water using sto3g**
python + cython:  0.0116 seconds, average over 1000 iterations
pure python: 0.0299 seconds,  average over 1000 iterations

on a new laptop(windows) pure python is 29 seconds for the big.molden. I was not able to get cython to run on windows. (old laptop has a linux partition)

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `0d34c7657af1`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.