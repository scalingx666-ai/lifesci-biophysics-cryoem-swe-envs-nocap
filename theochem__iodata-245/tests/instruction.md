# Issue

**Title:** Allow absence of orbital coefficients

## Description

This PR was initially intended to address a minor issue of the QChem log file loader, namely that it initialized an array full of `nan`s for the orbital coefficients, because the `MolecularOrbitals` class insisted on having this array. This PR fixes that limitation and allows `None` for the array of orbital coefficients. In addition, other attributes are also allowed to be None, without breaking functionality. There were no unit tests for the `MolecularOrbitals` class, so these are now included as well. The API remains fully backward compatible.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `08fa230e48a3`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.