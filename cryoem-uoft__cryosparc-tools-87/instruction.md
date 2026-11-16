# Issue

**Title:** CI: Windows and Artifact upload build fixes

## Description

- Updated to pypa/cibuildwheel@v2.17.0 to avoid Windows build crash
- Separate artifact uploads to keep names unique due to breaking changes in actions/upload-artifact@v4

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `0765418f77c7`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.