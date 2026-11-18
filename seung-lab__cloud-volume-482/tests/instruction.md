# Issue

**Title:** fix: update morton coding

## Description

For grid dimensions that are a power of 2, the code was attributing an extra bit.  Fixing that also seems to fix the prior problem of size=1 dimensions (a power of 2!) naturally, without the extra check.

I added a test for the case I recently hit, where the morton code was out of range (195, where the #chunks was 128).  In addition, a prior test was incorrect and updated.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `73ac95cf03a1`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.