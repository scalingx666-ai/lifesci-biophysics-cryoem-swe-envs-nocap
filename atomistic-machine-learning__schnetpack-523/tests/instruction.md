# Issue

**Title:** fix per_atom_output_key usage in Atomwise

## Description

I ran into the problem that the per_atom_output_key in the Atomwise module (https://github.com/atomistic-machine-learning/schnetpack/blob/3f6eecee4b35ea5bb6e5f883ae70b789f2bd3e75/src/schnetpack/atomistic/atomwise.py#L14) is not used in any way. I decided to fix the code.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `ea49834bc608`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.