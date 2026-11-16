# Issue

**Title:** Rework the deploy pipeline

## Description

Based on https://packaging.python.org/en/latest/guides/publishing-package-distribution-releases-using-github-actions-ci-cd-workflows/

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `359a2994a25d`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.