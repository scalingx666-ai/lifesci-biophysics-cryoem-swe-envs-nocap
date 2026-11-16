# Issue

**Title:** Test updated schema for adding Python 3.13 support.

## Description

## Summary by Sourcery

Add Python 3.13 support by updating CI test matrix and project metadata

Enhancements:
- Add Python 3.13 to the project's classifiers in pyproject.toml

CI:
- Include Python 3.13 in the GitHub Actions pytest matrix and add Python 3.10 and 3.11 to the strategy
- Restrict macOS and Windows runs to only Python 3.9 and 3.13

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `ab01d842e166`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.