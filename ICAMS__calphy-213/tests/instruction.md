# Issue

**Title:** Faster input

## Description

This pull request adds support for optional full validation during input parsing, enabling faster job submission workflows by skipping expensive validation when not needed. The changes introduce a `validate` parameter to input parsing functions and CLI commands, allowing users to choose between quick parsing and thorough validation. Additionally, the codebase is refactored for improved readability, and the project version is bumped to 1.5.3.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `f07f7d4680f1`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.