# Issue

**Title:** Enzyme silently dropping gradients workaround

## Description

As mentioned before, this is a workaround to get gradients working in some edge cases.

Underlying issue can be found here:
Fix can be found here, we can revert this when it gets merged:

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `48bbfa3414de`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.