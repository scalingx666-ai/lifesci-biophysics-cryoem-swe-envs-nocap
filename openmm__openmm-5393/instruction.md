# Issue

**Title:** Add CI on self-hosted runners

## Description

Adds CI for OpenCL and CUDA on self-hosted runners.

Note that like the existing Jenkins setup, these runs don't do the Python tests, but this should be possible to add if we want to.  Unlike the existing Jenkins setup, this doesn't add any CPU tests, since we already have many such configurations on GitHub runners, including ARM on Linux.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `c6173db6e8ed`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.