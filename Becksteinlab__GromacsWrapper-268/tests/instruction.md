# Issue

**Title:** updated CI environments

## Description

- remove six from all environments (should have been in PR #265, part of #259)
- add sphinx_rtd_theme to the RTD CI as it is apparently no longer installed automatically by RTD. (fix #267)

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `691393c1fb71`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.