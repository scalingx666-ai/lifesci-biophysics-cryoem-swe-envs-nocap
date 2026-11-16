# Issue

**Title:** PLUMED Collective Variables in OPS

## Description

This adds a PLUMED wrapper to the OPS collective variables, along with tests and examples. The wrapper requires a PLUMED and py-PLUMED (available via conda-forge) and OpenMM (available from conda/omnia). Tests are skipped in these requirements are not met.

Unsure about if/how to change OPS conda-recipe/meta.yaml

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `6e48b62edd70`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.