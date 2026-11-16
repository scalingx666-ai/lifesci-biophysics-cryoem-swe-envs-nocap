# Issue

**Title:** Use standard OpenCL C++ header

## Description

.  `opencl.hpp` is a standard header on all modern OpenCL distributions.  Apple still uses an ancient version that doesn't include it.  That's why we bundle a copy.  On all other platforms, we should use the standard one instead of the bundled one to avoid compatibility issues.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `9ff6e6945542`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.