# Issue

**Title:** Remove unnecessary array copy in `extract_trajectories_by_replica`

## Description

Return type of `StoredArrays._chunks` is incorrect; `chunk` here is already an array

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `5a5b17356972`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.