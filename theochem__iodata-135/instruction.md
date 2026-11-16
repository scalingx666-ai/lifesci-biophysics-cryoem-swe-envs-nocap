# Issue

**Title:** Remove GBASIS_CONVENTIONS

## Description

It was outdated and no longer useful because GBasis has its own wrapper for IOData.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `483caa35743e`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.