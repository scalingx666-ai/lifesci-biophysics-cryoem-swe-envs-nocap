# Issue

**Title:** improve automated CMake build system

## Description

This PR adds a `BUILD_FFTW` flag to build Tinker's included `fftw/` library. It should link the requisite libraries, based on compiler. (Intel includes FFTW headers in its compiler.)

Another added flag is `CMAKE_BUILD_TYPE`, which changes compiling flags from optimized to proper debugging flags.

I have not been able to test this on a MacOS or Windows machines, but I've transferred most flags from the Makefiles.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `286b109a62fc`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.