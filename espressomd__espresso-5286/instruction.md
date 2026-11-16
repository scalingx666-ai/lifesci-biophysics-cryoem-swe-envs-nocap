# Issue

**Title:** Fix regressions in the build system and FPE instrumentation

## Description

Description of changes:
- the Kokkos library was accidentally set up to enable hardware-dependent optimizations, which can cause trouble for HTCondor users, since they typically have access to computing resources with heterogeneous hardware
- compiler diagnostics are no longer emitted inside the Kokkos and Cabana libraries when included via the FetchContent mechanism, for consistency with the FindPackage mechanism
- FPE instrumentation is now temporarily disabled when calling third-party libraries known to raise exception flags
- FPE instrumentation is now disabled for features and test cases that explicitly rely on floating-point exceptions

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `74e1758d876e`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.