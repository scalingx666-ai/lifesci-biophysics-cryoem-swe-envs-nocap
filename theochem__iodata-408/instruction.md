# Issue

**Title:** Correctly parse fchk files without whitespace between values

## Description

The test file was generated with Psi4 and has been stripped down below 100k with a new tool `tools/stripfchk.py` included in this PR.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `76185a6554c3`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.