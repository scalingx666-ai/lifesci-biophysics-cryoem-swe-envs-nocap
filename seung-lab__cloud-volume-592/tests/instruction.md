# Issue

**Title:** feat: add cache multi-processing locking feature

## Description

Adds `CloudVolume(..., cache_locking=True)` flag (true by default). 

This starts using the new CloudFiles inter-process locking mechanism which uses `fasteners` to create locking files for local cache access but not for `file://` access generally.

This may have some performance implications, but can be switched off. It's only enabled for cache because otherwise the number of lock files will grow without bound. Caches get cleared periodically.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `7a72ba6b43f1`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.