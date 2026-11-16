# Issue

**Title:** feat: find methods multiple projects, workspaces, jobs

## Description

- New `find_projects`, `find_workspaces` and `find_jobs` methods for relevant controllers. `find_jobs` includes filtering arguments.
- Avoid sending now-unnecessary kill command

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `9b7ce0ab9ed5`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.