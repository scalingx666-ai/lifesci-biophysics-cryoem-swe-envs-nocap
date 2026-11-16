# Issue

**Title:** adding overload so GPU kernels execute without unsupported dynamic function call

## Description

Several GPU kernels are not functional because of an `unsupported dynamic function call to force(...)`. This adds a simple fallback for calling `force(..., weight_14)` as just `force(...)`. It seems to work on my GPU, at least.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `1f1443d73ff5`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.