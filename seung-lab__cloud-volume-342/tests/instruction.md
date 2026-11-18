# Issue

**Title:** perf: parallelized ShardReader.exists

## Description

Need to group the minishard requests more effectively.

@akhileshh needs this to make the graphene server more responsive.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `c3556ac9116f`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.