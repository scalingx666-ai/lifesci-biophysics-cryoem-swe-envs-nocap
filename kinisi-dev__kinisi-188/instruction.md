# Issue

**Title:** add`.pre-commit-config.yaml`

## Description

# Summary
- add `.pre-commit-config.yaml` and describe usage in the contribution guidelines
- exclude ruff formatting for docs
- replacing calls to ruff in CI with `- uses: pre-commit/action@v3.0.1` tested via c114146 and 0ff3c66

Notes
- [ ] should we format the docs as well or leave it out?
- [ ] I started using https://github.com/j178/prek over https://github.com/pre-commit/pre-commit but the later one is more established.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `5f2ce1cc4e0f`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.