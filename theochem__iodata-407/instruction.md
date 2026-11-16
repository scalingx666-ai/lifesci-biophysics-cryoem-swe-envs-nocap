# Issue

**Title:** Fix test and doc errors and warnings with most recent dependencies

## Description

This is just maintenance:
- Update pre-commit hooks
- Use modern Python approach to load IOData's version number in to sphinx
- Fix a warning when builkding the docs
- Replace some outdated pytest constructs in the unit tests
- Upgrade too low build-system dependencies
- Add the worksteal option to pytest for improved unit test concurrency

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `e6a606e985a4`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.