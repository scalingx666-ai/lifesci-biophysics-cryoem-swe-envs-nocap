# Issue

**Title:** Bump version to 0.0.68

## Description

## Summary

Version bump for the first release built by the new release workflow. The wheel will carry the web UI bundle built in CI.

## Technical description

`pyproject.toml` version 0.0.67 → 0.0.68. No other change. After merge, the `v0.0.68` tag on the merge commit triggers `release.yml`.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `8694c23c19da`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.