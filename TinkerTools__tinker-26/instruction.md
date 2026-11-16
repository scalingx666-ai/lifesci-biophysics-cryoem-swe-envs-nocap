# Issue

**Title:** Isokinetic

## Description

Fixed bug with openmp in ehal1.f and empole1.f that caused simulations to crash if they were compiled with openmp.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `014a69528841`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.