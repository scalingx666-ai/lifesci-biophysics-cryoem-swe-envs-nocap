# Issue

**Title:** Remove dependency on torch_cluster

## Description

This PR replaces every usage of the Distance module with the OptimizeDistance one.
I moved the old Distance module to the benchmarks/neighbors.py.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `a54a8a62de88`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.