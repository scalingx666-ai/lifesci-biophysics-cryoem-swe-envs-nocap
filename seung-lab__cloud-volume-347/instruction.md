# Issue

**Title:** perf: threaded shard get_data

## Description

- adds green flag to ShardReader.
- Now the shard_index_cache is always indexed by path instead of just the shard name.
- get_data is now threaded
- adds progress argument

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `984a9df6958c`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.