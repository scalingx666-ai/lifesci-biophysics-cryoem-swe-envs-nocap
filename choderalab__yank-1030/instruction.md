# Issue

**Title:** We don't need to create an nvcc-cache on cray machines

## Description

This PR removes some code I had thought was necessary for Cray machines, but turns out not to be.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `b4cdf6f718b0`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.