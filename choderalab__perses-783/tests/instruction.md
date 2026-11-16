# Issue

**Title:** Change default temp in protein mutation setup pipeline

## Description

Currently, the `temperature` is defaulted at 300 K, but it should be 298 K (as the default temperature for integrators in `openmmtools` is set to 298 K and 298 K is closer to 25 celsius).

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `959ed2ba7e83`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.