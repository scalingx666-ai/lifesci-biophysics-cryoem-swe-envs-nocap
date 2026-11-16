# Issue

**Title:** Disable CI failure if codecov fails

## Description

Quick change to ignore issues we hare having with codecov while we wait to hear from them:
https://community.codecov.com/t/too-many-uploads-to-this-commit-code-invalid/3384

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `abd5e62709f4`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.