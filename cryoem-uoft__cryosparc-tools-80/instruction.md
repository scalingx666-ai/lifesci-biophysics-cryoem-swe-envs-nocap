# Issue

**Title:** Fix: raise explicit memory error when dataset allocation fails

## Description

So that adding rows does not silently fail.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `9644af4b745e`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.