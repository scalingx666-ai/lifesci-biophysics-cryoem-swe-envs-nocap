# Issue

**Title:** Dropping macos-13 (legacy) tests in CI

## Description

## Description
This changes our current CI workflow in order to not run it on legacy macos-13 (intel). This is in order to reduce the load of our CI workflows by running less jobs, streamlining the PR changes a little bit.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `de075518df37`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.