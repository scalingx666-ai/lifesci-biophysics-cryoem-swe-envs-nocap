# Issue

**Title:** Fixes to extract trajectory

## Description

- Fix #848: Use MDTraj Trajectory.save() method instead of inferring function from extension.
- Fix #635: Allow extract-trajectory to handle trajectories with 1 frame.
- Fix #964: state index argument in extract_trajectory for SAMS calculations.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `12c6ec14ae8f`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.