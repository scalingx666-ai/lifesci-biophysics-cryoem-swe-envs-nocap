# Issue

**Title:** add automated deployment workflow

## Description

- fix #295
- automatically deploy to TestPyPi or PyPi (for releases) using the action MDAnalysis/pypi-deployment
- update CHANGES (ready for 0.9.1 release)

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `799f8b83b5b9`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.