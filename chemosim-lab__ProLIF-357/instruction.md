# Issue

**Title:** Ignore self interaction

## Description

Adds an `ignore` parameter to the run methods to bypass specific residue pairs (defaults to `lambda res1.resid == res2.resid`).
Mostly useful when calculating a residue interaction network.

TODO:
- [x] update changelog

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `50e95a0fb83b`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.