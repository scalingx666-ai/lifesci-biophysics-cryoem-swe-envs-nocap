# Issue

**Title:** fix: default to 'raw' for shard encodings

## Description

Looks like neuroglancer uses this default. Thanks for reporting @schlegelp! 

Related to #354

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `07ace8af61c1`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.