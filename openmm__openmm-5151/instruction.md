# Issue

**Title:** Minor optimization to PME

## Description

This avoids a few unnecessary atomic operations in charge spreading.  Running the standard benchmarks on RTX 4080, I get an overall speedup in the range of 1%.  Not a big deal, but still consistent improvement.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `a112fbfcc7da`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.