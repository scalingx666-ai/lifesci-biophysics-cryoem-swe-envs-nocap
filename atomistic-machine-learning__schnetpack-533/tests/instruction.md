# Issue

**Title:** raise Exception if array is passed for energy in calculator + typehint

## Description

Currently the `SPKCalculator` converts energy to float with `.numpy()[0]`.
If by accident one passes an array for energy (this should not be done) the calculator uses the first element and runs fine. I changed this to raising an exception for that case.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `46fbc5dd07d4`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.