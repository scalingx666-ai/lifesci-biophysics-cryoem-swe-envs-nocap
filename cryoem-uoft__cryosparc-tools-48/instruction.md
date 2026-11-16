# Issue

**Title:** Release v4.3.1

## Description

- Fixed: `Job.queue` method `lane` argument is now optional when queuing directly to master
- Fixed: Dataset memory leak due to missing reference count decrease during dataset garbage collection
- Docs: pip update instructions
- Docs: Recommend Python environment as a prerequisite

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `f856911b41a2`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.