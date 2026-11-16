# Issue

**Title:** Update GitHub Actions versions

## Description

Attempt to address the failing PyPI release.

## Summary by Sourcery

Update release workflow to use Python 3.12 and Sigstore action v3 to fix PyPI release failures.

Enhancements:
- Use Python 3.12 in the GitHub Actions release workflow
- Upgrade the Sigstore GitHub Action to v3

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `cd685937202b`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.