# Issue

**Title:** feat: agglomerating entire blocks of proofreading

## Description

This PR does two tasks:
1. Support the legacy (1.0) and new (api/v1) style requests to graphene servers.
2. Provide a means for downloading flat segmentation at a given time-point from graphene. 

This is provided by the `agglomerate=True` and `timestamp=XXXX` flags on the `cv.download` method.

The agglomerate flag will work on both api/v1 and 1.0 endpoints, but it will only work practically on the api/v1 endpoints due to the implementation of "get_roots".

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `17adbe06e68b`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.