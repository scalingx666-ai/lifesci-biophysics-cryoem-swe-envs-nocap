# Issue

**Title:** Add `LennardJonesSoftCore` and `CoulombSoftCore`

## Description

This PR adds soft core version of the Lennard jones and Coulomb pairwise interactions following the expressions in [Beutler, T. C.; Mark, A. E.; van Schaik, R. C.; Gerber, P. R.; van Gunsteren, W. F. Chem. Phys. Lett. 1994, 222, 529−539](https://doi.org/10.1016/0009-2614(94)00397-1).

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `5c72eefcb827`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.