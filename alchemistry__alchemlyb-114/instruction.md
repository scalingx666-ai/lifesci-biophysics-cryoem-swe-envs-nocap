# Issue

**Title:** Implement the workflow pipeline

## Description

I will try to Implement the workflow pipeline as is discussed in #111 .

I noticed that the subsampling part will be refactored with #98. Given I cannot leave this part blank, I have made a minimal implementation of the corresponding option and I'm happy to change it when #98 is merged.

All the information is logged using logging #34.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `5828b40b107b`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.