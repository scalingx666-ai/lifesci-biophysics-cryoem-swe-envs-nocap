# Issue

**Title:** Implement cell optimisations through ASE

## Description

Add a new `ase` module under `sampling` to perform cell optimisations.

We still need:
- documentation
- better exit handling upon timeout / nonconvergence

Currently, it is implemented alongside the IPI `optimize` module. In time, we should decide whether both modules are really needed. This merge is mainly to avoid the headache of having to juggle multiple branches for installations.

To avoid confusion `from psiflow.sampling import optimize` has been removed in favour of explicitly importing from `psiflow.sampling.optimize` (IPI) or `psiflow.sampling.ase` (ASE).

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `f2bd5e5e699c`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.