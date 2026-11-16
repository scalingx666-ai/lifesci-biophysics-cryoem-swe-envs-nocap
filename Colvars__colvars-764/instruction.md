# Issue

**Title:** Add ARM64 apptainer container definition file

## Description

I am able to build the SIF image locally but not sure how to publish it on https://github.com/colvars/colvars/pkgs/container/devel-containers. @giacomofiorin Any ideas?

In addition, the ARM64 version of libtorch is not available on Linux.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `f2994c32eba7`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.