# Issue

**Title:** Fix bug solid

## Description

This pull request includes a version update for the `calphy` package and a minor change in the `run_minimal_averaging` method in `calphy/solid.py`. The most important changes are grouped below:


### Code Simplification:
* Simplified the `run_minimal_averaging` method in `calphy/solid.py` by removing the redundant `species` argument when calling `ph.create_structure`.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `19411b5a1917`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.