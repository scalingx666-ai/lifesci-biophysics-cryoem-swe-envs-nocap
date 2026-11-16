# Issue

**Title:** Rtp patch

## Description

Patch to make the rtp parser play nice with modifications. Essentially, we need to add updated resids to modification atoms. I also added a fail-safe in molecule resid assignment upon merging.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `fd4f0b839107`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.