# Issue

**Title:** Interoperate with alchLambdaFreq in NAMD

## Description

alchLambdaFreq is a yet undocumented NAMD feature that makes the alchemical lambda evolve according to a linear ramp over time. This change set ensures compatibility of that feature with Colvars:
- prevents conflicts between the ramp and lambda-dynamics from Colvars
- fetches the up-to-date value of lambda when using linear ramps.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `1ee3ec105b6c`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.