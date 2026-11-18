# Issue

**Title:** Image shard generation

## Description

Shard Write:
* fix: don't crop partially intersecting shards
* feat: Add progress flag to `make_shard`
* perf: Avoid repeated string concatenation

Shard Read:
* perf: Avoid copies when converting JPEG bytes to numpy array
  * 30ms -> 0.5ms per call
* perf: Bundle adjacent byte range requests
  * 1000 s -> 5 s for completely empty/black shard
  * 1 s -> 0.5 s for example in

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `a204dcc496f2`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.