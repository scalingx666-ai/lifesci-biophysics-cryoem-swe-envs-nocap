# Issue

**Title:** Early exit from short range kernels (cabana and classic)

## Description

This exits early from the short range kernels, when particles are further apart than the maximal cutoff (non-bonded AND others)
wq
As a result, slightly larger skins are possible. The Lj benchmark is faster by some 10%, for P3M it's less.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `70d3ea8671f5`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.