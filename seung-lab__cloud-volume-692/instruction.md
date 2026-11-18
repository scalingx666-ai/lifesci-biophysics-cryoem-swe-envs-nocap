# Issue

**Title:** perf: faster all label query with psql

## Description

It turned out the fastest approach to query all the unique labels with psql is to dump all the labels then dedup with unique, it reduces processing a data containing 2B unique labels from 4 hours to 15 minutes. I think this is because the labels are mostly unique, only the very large segments can span multiple chunks, so it is not efficient to use complex database query to jump over redundent labels.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `9e8d1993375f`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.