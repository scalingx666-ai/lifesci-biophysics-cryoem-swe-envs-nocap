# Issue

**Title:** Add CI to check if tutorials can be run.

## Description

This PR adds a github action to run all tutorials.

Q: what should the trigger for this be? Not sure if we should run this on every push/pull request.

Might be sufficient if we run this when a PR is marked with `prepare_release` or something.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `fd3e2b27faea`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.