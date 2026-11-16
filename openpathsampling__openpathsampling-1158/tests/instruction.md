# Issue

**Title:** Storage Interfaces

## Description

These are intended to abstract out filesystem storage to be an arbitrary key-value store. As examples, I've implemented storage on the filesystem and in memory. Object storage (e.g., S3) can be implemented separately.

I'm making this a separate PR so that it can be used in future PRs.

I'd also appreciate suggestions on a better name! `StorageHandler` is kind of awful. [UPDATE: Changed to `StorageInterface`]

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `5b0828470b3f`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.