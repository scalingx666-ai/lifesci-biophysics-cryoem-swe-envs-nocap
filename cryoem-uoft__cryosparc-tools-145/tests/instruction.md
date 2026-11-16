# Issue

**Title:** fix: encode pydantic models with JSON mode

## Description

Instead of python object mode. Fixes circular reference error when used on some models.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `aeb7f4c5ddb2`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.