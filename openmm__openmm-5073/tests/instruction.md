# Issue

**Title:** Update constant potential entry in user guide

## Description

Emphasizes that it is incorrect to set non-zero charges in a NonbondedForce when using constant potential, and adds a note about running constant potential simulations with polarizable models.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `4507205cf23d`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.