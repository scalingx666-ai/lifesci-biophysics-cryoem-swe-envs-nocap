# Issue

**Title:** Allow the user to specify a seed for srand

## Description

Previously, simulations could not be replicated in a practical manner due to the program selecting a random-enough seed each time.  This adds the `--srand` option, which allows the user to specify a seed.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `4a17e1730a30`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.