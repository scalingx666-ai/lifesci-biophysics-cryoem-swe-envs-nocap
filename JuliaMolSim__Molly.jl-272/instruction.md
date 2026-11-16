# Issue

**Title:** Add support for Double Exponential Potential

## Description

Extended force field parsing logic to read and load double exponential potentials. This is done by literal string matching, the string must be strictly compliant with a valid OpneMM string for the same functional form.

On top of the normal double exponential, I have also added the soft-core variant, following the `alpha` and `beta` mixing logic found in https://doi.org/10.1039/d3dd00070b

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `4fc5518d7742`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.