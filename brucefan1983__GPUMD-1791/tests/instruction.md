# Issue

**Title:** Add Wisevolve Potential Engine C API for GPUMD

## Description

### Summary

Add optional support for the Wisevolve Potential Engine (WPE) as an external potential backend for GPUMD.

### Changes

- Add the public WPE C API and optional hooks in the GPUMD force/run paths.
- Add a `gpumd-wpe` build target while keeping the standard GPUMD build unchanged by default.
- Build the WPE adapter from an external WPE repository and link against `libwisevolve_potential.so`.
- Add API consistency checks and runtime path handling through `WPE_SO_ROOT`.

### Validation

- Verified against the current GPUMD master with no unrelated upstream changes modified.
- Confirmed successful WPE-enabled compilation and linking with CUDA.
- Checked stock/WPE build switching, API-header consistency, and runtime library resolution.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `ce1ad87f238c`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.