# Issue

**Title:** Add load_one and load_many functions for Gromacs gro files.

## Description

This PR is associated with #37 to support Gromacs gro files. I take most everything for molmod without many modifications, including some units that Gromacs use by default (nanometers and picoseconds).

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `5ff075043e66`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.