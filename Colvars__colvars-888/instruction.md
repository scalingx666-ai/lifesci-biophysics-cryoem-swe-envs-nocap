# Issue

**Title:** build: guard the CUDAGM interface by NAMD_CUDA and NAMD_HIP macros

## Description

Only build the plugin when NAMD_CUDA or NAMD_HIP is defined. This commit is a step to integrate the plugin in the NAMD's build scripts.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `733f0b57405e`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.