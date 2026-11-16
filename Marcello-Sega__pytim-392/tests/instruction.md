# Issue

**Title:** FIX: set empty atom groups for WC

## Description

The Willard-Chandler method does not identify surface atoms or layers, so these are now set to empty atom groups. An additional comment has been added to the documentation.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `154edc4ea56d`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.