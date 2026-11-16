# Issue

**Title:** Add environment variable for default scratch directory.

## Description

This is a simple addition to the gui to pickup the environment variable RELION_SCRATCH_DIR and use it for all 5 jobs that use scratch directories.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `6dddc66643be`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.