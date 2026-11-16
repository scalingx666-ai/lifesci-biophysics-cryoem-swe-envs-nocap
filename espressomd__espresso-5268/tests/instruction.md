# Issue

**Title:** More fully connected boundaries

## Description

Claude Code found a 2nd bug which we missed, as we only tested cubic boxes.

This PR has the fix and tests for this and the previous bug.

- **Fix fully connected boundaries coord mix-up**
- **Additional testing for fully connected boundaries**

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `c1e1e0369ad5`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.