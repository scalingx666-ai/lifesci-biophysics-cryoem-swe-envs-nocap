# Issue

**Title:** Optimize the GPU pairlist of the coordnum kernel

## Description

This PR follows the same strategy described in  and changes the GPU pairlist of `coordnum` from an array of `bool` to an array of bit fields. **This PR depends on  and

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `e30f2fc68501`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.