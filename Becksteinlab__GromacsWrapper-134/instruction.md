# Issue

**Title:** properly handle bytes output from subprocess in auto tool detection

## Description

- fix #133
- affected macOS with Python 3
- added test

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `37948674b1e0`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.