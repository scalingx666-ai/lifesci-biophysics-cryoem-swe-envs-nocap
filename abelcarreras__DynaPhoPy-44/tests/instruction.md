# Issue

**Title:** Switch to uv and fix Phonopy Symmetry issue

## Description

Main change:
- Switch to uv
- Add a gitignore
- Modify phonopy Symmetry object that fails with new phonopy version

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `52d80535214f`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.