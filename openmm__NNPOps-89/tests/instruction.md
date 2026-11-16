# Issue

**Title:** Move all torch.ops.load calls to the __init__.py scripts

## Description

This PR tries to fix #68 by moving all calls to torch.ops.load to the __init__.py scripts.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `c2f0ca043a44`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.