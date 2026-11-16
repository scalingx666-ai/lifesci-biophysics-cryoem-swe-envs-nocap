# Issue

**Title:** Improve OIF documentation and testing

## Description

Description of changes:
- move OIF API documentation from the user guide to Python docstrings
- document OIF viscous damping functionality
- remove unused or commented out OIF code
- add OIF package to the Sphinx module tree
- add a unit test for OIF local forces and utility functions

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `9c1a379ea35e`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.