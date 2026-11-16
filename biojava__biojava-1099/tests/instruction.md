# Issue

**Title:** Removing functionality that uses mmtf.rcsb.org, now deprecated

## Description

This removes any functionality that uses files from the mmtf.rcsb.org API, now deprecated by RCSB PDB.

It is still possible to read/write MMTF files from elsewhere.

Also: demoted logging level when content-length size can't be retrieved (happens for bcif retrieval from models.rcsb.org)

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `2274ea303440`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.