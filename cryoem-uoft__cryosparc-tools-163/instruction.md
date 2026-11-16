# Issue

**Title:** fix: update workspace and session type signatures

## Description

Endpoints that return workspaces always unambiguously say that they can return both a session and a workspace model.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `96912dfb79d7`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.