# Issue

**Title:** Simplify and optimize coordination numbers

## Description

This PR contains several improvements:
- [x] Condense the computation of multiple variants of coordination numbers: all classes are now derived from `coordnum` with the exception of `h_bond`
- [x] Separate the pairlist update from the switching function computation
- [x] Skip computing the switching function if pairwise distance is too large for the given tolerance
- [x] Optionally, allow using an inline function to compute PBC distances instead of a virtual function

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `9bc891ba5eee`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.