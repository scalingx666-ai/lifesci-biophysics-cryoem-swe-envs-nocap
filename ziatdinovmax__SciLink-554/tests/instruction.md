# Issue

**Title:** Bump version to 0.0.69

## Description

## Summary

Version bump for the release carrying the sidebar delegation tree and Telemetry tab, multi-user hardening, and the MCP tab.

## Technical description

`pyproject.toml` version 0.0.68 → 0.0.69. No other change. The `v0.0.69` tag on the merge commit triggers `release.yml`.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `11dc20cb1d9a`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.