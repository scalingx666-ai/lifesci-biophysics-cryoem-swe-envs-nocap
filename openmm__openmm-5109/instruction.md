# Issue

**Title:** Fix issue with constant potential and large neighbor lists

## Description

Fixes an issue where `ConstantPotentialForce` could produce incorrect charges on the GPU platforms in cases where the neighbor list is too small and has to be resized.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `2b20ffbabc40`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.