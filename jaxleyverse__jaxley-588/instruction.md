# Issue

**Title:** add automatic release workflow

## Description

first draft of a release workflow with the following idea:

once a PR is labelled with `release`, it runs additional checks if version numbers have been incremented and the changelog has been updated. On push to `main`, we tag the commit, build the wheel, publish it to pypi and make a github release with the latest changes.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `8773b9d4bcf3`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.