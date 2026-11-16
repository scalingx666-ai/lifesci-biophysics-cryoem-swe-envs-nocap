# Issue

**Title:** Update GitHib Action to test on Python 3.14

## Description

This is a follow-up on #387, now enabling testing on Python 3.14.

## Summary by Sourcery

Enable testing on Python 3.14 by updating the GitHub Actions workflow matrix to include explicit OS and Python version combinations and adjust the oldest dependencies installation step to target Python 3.10.

CI:
- Restructure pytest GitHub Actions matrix to explicit include combinations
- Add Python 3.14 to tests across Ubuntu, macOS, and Windows
- Update oldest dependencies installation to run on Python 3.10 instead of 3.9

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `b0adae8398d7`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.