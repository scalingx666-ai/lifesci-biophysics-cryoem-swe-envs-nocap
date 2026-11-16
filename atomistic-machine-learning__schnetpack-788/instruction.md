# Issue

**Title:** Callback & Resume Training Fixes

## Description

## Changes

- Add the progress bar callback to the training configuration.
- Ensure resumed training restores the correct callback and training state.
- Added black version in setup file

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `11f6f71922bf`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.