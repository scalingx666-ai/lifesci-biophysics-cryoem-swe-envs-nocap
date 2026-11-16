# Issue

**Title:** dpd_pair_force without particle params

## Description

The `dpd_pair_force function()` currently takes two references to `Particle` objects as input parameters. Due to a future change in the data model of `Particle` structs, this function should rather take references to data members.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `608ecf1f7c9b`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.