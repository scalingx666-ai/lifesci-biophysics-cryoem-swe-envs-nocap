# Issue

**Title:** perf(spatial_index): replace orjson with pysimdjson and only parse keys

## Description

Initial tests seem to shrink the share of compute for JSON parsing dramatically.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `8100b8551646`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.