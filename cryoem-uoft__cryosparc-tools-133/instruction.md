# Issue

**Title:** feat: additional properties for controllers

## Description

- title and description properties for projects, workspaces, jobs
- params, input, outputs properties for jobs
- `.dir` property may be used without calling (which is deprecated)

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `9af77c6c6ba5`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.