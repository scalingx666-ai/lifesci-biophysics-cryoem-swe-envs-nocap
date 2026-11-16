# Issue

**Title:** Disable caching on venv tests

## Description

The caching in Github Actions seems to break easily. (This is probably due to limited storage for the caches, not sure.) I'm keeping cache in place for the conda test on the master branch, but not for venv tests which run for each PR.

I'll merge this as soon as tests pass because this issue affects all other PRs.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `ec14a919423e`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.