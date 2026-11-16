# Issue

**Title:** Update ci.yml to use v3 of Actions

## Description

When merging the last pull request on `iodata` I got an warning message:  

`Node.js 12 actions are deprecated. Please update the following actions to use Node.js 16: actions/checkout@v2, actions/setup-python@v2. For more information see: https://github.blog/changelog/2022-09-22-github-actions-all-actions-will-begin-running-on-node16-instead-of-node12/.`

The fix seems to be to upgrade to version 3 of actions and I made the appropriate increment.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `5a4f58069369`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.