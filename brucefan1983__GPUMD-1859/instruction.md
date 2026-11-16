# Issue

**Title:** extend some interfaces

## Description

### Summary

Extend shared NEP interfaces to reduce changes to existing code in #1851.

### Changes

- Allow derived models to reuse parameter reporting and override ZBL reading.
- Separate model header and parameter writing.
- Support extended restart parameter counts and optional energy-reference output, preserving fine-tune handling.

### Validation

Full NEP regression suite: **24 passed, 0 failed**.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `334ac0d38918`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.