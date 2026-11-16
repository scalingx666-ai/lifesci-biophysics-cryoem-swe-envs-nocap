# Issue

**Title:** Tetrahedral numerical differentiation

## Description

In 3d graphics, gradients are often calculated by sampling the 4 points in a tetrahedron around the desired point instead of 6 points in an octahedron around the desired point.  This commit adds tetrahedral numerical differentiation as an option.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `db01ac04a5ef`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.