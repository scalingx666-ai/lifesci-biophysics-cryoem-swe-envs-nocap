# Issue

**Title:** core/pressure: fix DPD virial dropping excluded pairs

## Description

Move the DPD force accumulation inside the particle exclusion loop to mirror the DPD pressure exclusion.

Fixes bug #7 of the adversarial core bug sweep.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `6f458e6017da`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.