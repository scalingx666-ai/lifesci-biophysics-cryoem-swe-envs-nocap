# Issue

**Title:** Functions

## Description

This PR enables you to evaluate individual geometries using `Function`s.

New behaviour:
- `Function()` for a single geometry, returning a `dict` with `outputs`
- `Function.compute()` for a list of geometries (a la `Hamiltonian`) which replicates the current behaviour of `Function()`

For users, nothing should have changed.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `2b0d72d66532`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.