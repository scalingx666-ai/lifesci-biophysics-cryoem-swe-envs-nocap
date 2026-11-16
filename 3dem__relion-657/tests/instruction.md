# Issue

**Title:** PORT: bring FFT stuff for the projection to GPU

## Description

This PR:
* performs FFT on GPU for the Expectation setup part
* fixes the issue discussed here 
* fixes several related CUDA kernels in terms of not taking predefined constant for the block size.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `87c294c2783a`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.