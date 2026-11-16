# Issue

**Title:** update pypi upload trigger

## Description

I think we want to use "release" rather than "published" for the event type in order to upload to pypi.

https://docs.github.com/en/actions/reference/events-that-trigger-workflows#release

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `8427f70d8edc`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.