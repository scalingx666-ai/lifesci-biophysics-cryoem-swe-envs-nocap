# Issue

**Title:** Update pyproject.toml

## Description

lightning versions 2.6.2 and 2.6.3 were found to contain credential stealing code on install.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `d10bd8cbe86e`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.