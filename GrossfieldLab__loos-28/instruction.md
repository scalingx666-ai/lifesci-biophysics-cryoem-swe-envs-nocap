# Issue

**Title:** ESP mesh tool

## Description

This PR adds a tool called esp_mesh to fit charges on a mesh composed of spherical shells to reproduce the electrostatic potential due to solvent around a solute. The new tool is in Tools/. The change to src/loos_defs.hpp declares an additional LAPACK routine, dgels, that I needed to fit the charge magnitudes by linear regression.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `ce45315e1401`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.