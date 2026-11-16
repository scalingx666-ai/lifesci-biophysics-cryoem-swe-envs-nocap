# Issue

**Title:** Adds Example for Running RBFE pair

## Description

* Removes `rbfe_edge_list.py` as it is not used in practice and differs from our best practices
* Adds `run_rbfe_legs.py` to simulates legs for a pair of molecules. Intended as a summary of our best practices. The initial pass covers the most obvious parameters to adjust, though over time we may want to add more.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `b0f53ccd4c04`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.