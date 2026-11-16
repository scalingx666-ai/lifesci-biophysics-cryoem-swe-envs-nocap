# Issue

**Title:** New tool poisson_integrator_conv

## Description

Useful to investigate the convergence of Poisson integration.
Contains small improvements to the existing standalone integration tool, eg: 3D files are written to DX format.
Adds a constructor of colvar_grid_scalar from a filename (reading a multicol file) which will be useful for future improvements.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `6cf0b4467f11`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.