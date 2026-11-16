# Issue

**Title:** Quick test with libcpptraj docker image

## Description

This can be configured to pull any version of libcpptraj to build
against so that we can test with an arbitrary cpptraj pull request.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `4c3d6451e5bb`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.