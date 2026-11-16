# Issue

**Title:** Fix bugs in MD module

## Description

Added several bug fixes for the MD module:
- Fixed a shape problem when logging stress tensors with the `FileLogger`.
- Extended automated activation of stress computation in `activate_model_stress` to the `Response` output module.
- Moved wrapping of positions for periodic systems out of the `RemoveCOMMotion` hook into a new `WrapPositions` hook.
- Periodic boundary condition array in system is now properly converted to Boolean type upon loading a state dict.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `6b98a857d470`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.