# Issue

**Title:** Enable PyTorch flags when building NAMD

## Description

Enable building of NAMD with the PyTorch interface, supported by NAMD `main`.

Standard LAMMPS currently uses PyTorch through [its Python interface](https://docs.lammps.org/pair_mliap.html), but does not link or load the library directly.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `9e9e3d973443`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.