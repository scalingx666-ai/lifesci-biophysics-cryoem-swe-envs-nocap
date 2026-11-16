# Issue

**Title:** Float precision of energies

## Description

reduce precision error in energies by subtracting reference energies in float64 before converting to float32

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `4601b54127d6`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.