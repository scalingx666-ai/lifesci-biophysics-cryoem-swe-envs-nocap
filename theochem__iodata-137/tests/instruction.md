# Issue

**Title:** Fix pylint issues

## Description

This PR also stops cardboardlint from filtering out issues related to code that was not changed in a PR. This change will reduce the probability to have failures on the master branch, but it may sometimes result in errors in the PR for things that are unrelated to the PR, e.g. when a newer version of pylint can detect new style issues.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `4c3f5db8591c`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.