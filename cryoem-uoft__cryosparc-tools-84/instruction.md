# Issue

**Title:** Fix: correct Dataset.descr() implementation

## Description

Make sure `exclude_uid` param works. Reported by Harris.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `b0d1e4a54200`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.