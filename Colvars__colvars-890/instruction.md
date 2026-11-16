# Issue

**Title:** Copy Colvars CUDA source code to all supported platforms

## Description

To ensure that (i) NAMD can create the same Make.depends files across CPU and GPU builds, and (ii) the source code of Colvars to be MD engine-agnostic, I end up with copying the CUDA source code to all supported platforms, even if the code are not actually compiled (guarded by `!defined(COLVARS_CUDA) && !defined(COLVARS_HIP)`.

This PR depends on  and should wait for  to be merged.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `54ae9ab4797c`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.