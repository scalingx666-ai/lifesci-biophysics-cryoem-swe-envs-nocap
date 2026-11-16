# Issue

**Title:** Add PyPI release workflow

## Description

This is another step to address #313.

The included workflow will make releases on PyPI and test releases on test.pypi.org.

I'm using an updated version of the trusted publisher workflow from: https://packaging.python.org/en/latest/guides/publishing-package-distribution-releases-using-github-actions-ci-cd-workflows/
(This documentation has some outdated versions of the relevant GitHub actions, which was easy to fix.)

We will only properly see this in action (and validate that it works) after the pull request is merged into main, because that is the branch from which releases are made.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `486e4137ab90`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.