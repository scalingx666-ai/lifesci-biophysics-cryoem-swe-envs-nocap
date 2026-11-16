# Issue

**Title:** Restore dropped pycodestyle, Pyflakes rules in ruff

## Description

In the transition to ruff (#1459), we unintentionally dropped the Pyflakes linting rules (e.g. unused imports) that were previously handled by `flake8`. This PR adds them back in by selecting the `F` rules in the ruff configuration ([reference](https://docs.astral.sh/ruff/linter/#rule-selection)).

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `9cd41bd11362`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.