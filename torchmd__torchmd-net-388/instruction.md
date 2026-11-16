# Issue

**Title:** fix triton fusion

## Description

This change fixes a bug where triton compilation can do some strange, incorrect fusion attempts inside the neighbors_brute kernel that results in indexing errors. The passing of neighbors[0], neighbors[1] seems to confuse the compiler, It is better to pass the full tensor and do the pointer math inside the triton code. This change is purely cosmetic when run in eager mode.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `bba7351beb99`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.