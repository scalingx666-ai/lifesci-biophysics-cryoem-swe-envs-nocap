# Issue

**Title:** ReaxFF CPU Optimizations

## Description

This PR improves ReaxFF CPU performance through three main changes:

- fusing hydrogen bond evaluations.
- avoiding atomic updates on the parallel EEM cpu path by implementing per-workgroup counts and write positions during csr construction.
- tuning workgroup sizes for sparse mat-vec multiplication, and enabling SIMD for row sums.

In local benchmarks using 32 julia threads, these changes reduced simulation time per step by ~40% for the systems tested.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `48bbfa3414de`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.