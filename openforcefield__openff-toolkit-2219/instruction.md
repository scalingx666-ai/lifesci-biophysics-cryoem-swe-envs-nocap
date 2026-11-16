# Issue

**Title:** Update deployment testing envs

## Description

This gets us back to the "sporadically failing" state of deployment tests
- [x] Manually add ambertools to deployment testing envs now that it's not brought in by openmmforcefields

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `7911500eea66`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.