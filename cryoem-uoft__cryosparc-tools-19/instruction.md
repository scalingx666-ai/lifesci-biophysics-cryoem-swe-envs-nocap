# Issue

**Title:** Release v4.1.3

## Description

- fix: error when creating a job with `CryoSPARC.create_job`
- fix: Dataset allocation error due to slot generation overflow
- docs: minor fixes

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `9ef6165f31d4`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.