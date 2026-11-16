# Issue

**Title:** Pin cerberus to 1.1

## Description

It will be a much larger pain to convert to 1.2 as there are a few things
which changed. Until we move to the public schema, I don't think its worth
the time sink to fix this proper.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `1aa436a9f909`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.