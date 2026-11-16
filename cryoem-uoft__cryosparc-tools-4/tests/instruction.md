# Issue

**Title:** fix: correct sdist build during publish

## Description

Adds a separate "lint" workflow step that also makes the sdist tarball

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `59478476c5a4`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.