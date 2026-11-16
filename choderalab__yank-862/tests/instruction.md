# Issue

**Title:** Decouple number of replicas and states for Reporter

## Description

This PR makes minor changes to `Reporter` so that an arbitrary number of replicas can be used.

This is the first step in implementing the SAMS sampler, but it useful to review and merge on its own.

This PR also contains some minor refactoring to reduce duplicated code, leading to overall -13 lines!

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `990aa4b1b80c`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.