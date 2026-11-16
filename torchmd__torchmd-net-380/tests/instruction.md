# Issue

**Title:** ASE calculator checks for charge changes

## Description

The base ASE calculator is responsible for checking if anything has changed, otherwise cached energy and forces are returned instead of being re-calculated. We use the atoms.info dict to store the total charge which does not get checked for changes. This PR extends the base calculators check_state to check for changes in the info dict.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `6d6be1efa676`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.