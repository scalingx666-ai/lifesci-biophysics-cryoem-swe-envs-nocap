# Issue

**Title:** feat: improved CryoSPARC class URL initialization

## Description

- Use first available signed-in URL if not specified
- Use `http://` protocol if not specified

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `736fab10a870`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.