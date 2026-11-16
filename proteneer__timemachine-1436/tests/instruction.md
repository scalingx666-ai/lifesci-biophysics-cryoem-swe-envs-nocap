# Issue

**Title:** Fixes failing water sampling nightlies

## Description

Failures introduced in #1431 by unwrapping the SummedPotentials.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `7f188b13e7c2`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.