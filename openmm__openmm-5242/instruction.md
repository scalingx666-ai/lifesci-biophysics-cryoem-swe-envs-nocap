# Issue

**Title:** Avoid precision loss in minimizer energy comparison

## Description

.  Computes the energy difference on the CPU first before rounding and sending to the GPU.

I cannot think of an easy way to come up with a test case that would have failed without this fix, as whenever I have seen this failure, it is stochastic.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `13c568d08d5a`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.