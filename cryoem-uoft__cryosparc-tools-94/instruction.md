# Issue

**Title:** Fix: correct pre-commit lint config

## Description

- Update pre-commit config
- Ensure ruff check doesn't only check imports
- Fix various type issues

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `440607c5de21`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.