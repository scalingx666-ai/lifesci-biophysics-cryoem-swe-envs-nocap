# Issue

**Title:** fixed Py2/Py3 bug in scaling.scale_impropers()

## Description

- use explicit integer division
- fix #132

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `6b87aa8aab29`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.