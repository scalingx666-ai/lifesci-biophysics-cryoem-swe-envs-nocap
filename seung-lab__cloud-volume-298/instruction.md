# Issue

**Title:** perf: cache shard and minishard indices

## Description

With default settings assuming 9 bits for each in the spec:
Shard index cache will take up about 4MB (512 entries)

Minishard index cache will take up potentially more... (128 entries) It is a bit less useful than the shard index cache, but for example, I ran across a wild example of a spec with 0 shard bits and many repeated accesses to the same minishards. A small cache will speed up odd cases like this.

Introduces new dependency on `lru-dict`

Partially addresses #264

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `149d965e8aae`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.