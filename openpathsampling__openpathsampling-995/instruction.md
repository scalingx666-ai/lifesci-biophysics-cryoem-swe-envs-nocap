# Issue

**Title:** SimStore storage.cvs should only be CVs

## Description

Previously, `storage.cvs` contained all the storable functions. It should only include CVs (which are specifically storable functions that take a snapshot as input). This PR fixes that issue.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `d303ff607326`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.