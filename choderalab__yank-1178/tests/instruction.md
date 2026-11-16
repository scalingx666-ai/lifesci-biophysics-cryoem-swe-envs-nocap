# Issue

**Title:** Fix #917: CLI command to setup simulations only

## Description

Fix #917: Add a `--setup-only` flag to the `yank script` to run only the automatic setup pipeline and skip the free energy calculation.

## Status
- [x] Ready to go.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `c18f5a9f1a82`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.