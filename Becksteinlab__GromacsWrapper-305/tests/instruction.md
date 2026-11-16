# Issue

**Title:** updated CI/CD deployment workflow

## Description

- directly use pypa workflow (instead of MDAnalysis deployment workflow)
- use different steps for build and deploy
- introduce environments testpypi and pypi
- test before upload

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `0052c5b6ef3e`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.