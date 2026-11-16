# Issue

**Title:** Improve NVHPC toolchain support

## Description

Fix double-free in prvalue temporary objects passed as const lvalue reference to functions arguments (affects nvc++ <= 26.3). Select host code compiler flags based on the host compiler id. Address NVHPC diagnostics.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `4e12791da7a3`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.