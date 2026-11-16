# Issue

**Title:** Fix neighbourlist gradient calculation on CPU

## Description

This aims to fix #84 

Adding `distances.clone()` (copying the existing `deltas.clone()`) stops autograd complaining about the in-place operation when using pytorch==1.13.

Also changed the `test_neighbor_grad` test so that it runs on CPU and GPU separately (so it can run on Github Actions CI) and compares the NNPOps gradients with reference gradients calculated using pytorch autograd.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `c2f0ca043a44`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.