# Issue

**Title:** Updating macos-13 runner to macos-15-intel

## Description

macos-13 runners (for x86-64) are closing down December 4th, 2025
https://github.blog/changelog/2025-09-19-github-actions-macos-13-runner-image-is-closing-down/

This pushes all our runners to macos-15-intel, which will be the last macOS x86-64 runners (since Apple dropped support for x86-64). These runners will last until Fall 2027.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `54f49a9d914d`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.