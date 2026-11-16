# Issue

**Title:** Fixes conditional that triggered warning incorrectly

## Description

* If steps_per_frame was greater than the water sampling interval would post a message, which is incorrect

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `433e071c46f9`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.