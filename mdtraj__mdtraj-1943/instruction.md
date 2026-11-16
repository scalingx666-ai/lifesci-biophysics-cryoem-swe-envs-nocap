# Issue

**Title:** RDF Reference for NumPy 2

## Description

This PR regenerates the RDF reference files using NumPy 2.0 and rolls back the RDF calculation function from the current NumPy 2 branch.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `4ca025adbdbe`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.