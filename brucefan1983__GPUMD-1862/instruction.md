# Issue

**Title:** rename header

## Description

### Summary

Rename `xyz_frame_splitter.h` to `.cuh` and update its include in `dump_xyz.cuh` to follow the project's header naming convention.

### Changes

### Validation

Validation: GPUMD regression tests passed.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `5bea807a4dc6`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.