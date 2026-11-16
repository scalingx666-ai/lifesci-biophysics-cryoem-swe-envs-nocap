# Issue

**Title:** bugfix: AFDB url is now retreived from their API.

## Description

The AlphaFold database recently changed their latest structure model version and removed the v4 structures.

In this PR, the code has been fixed to use the API to retrieve the URL of the latest PDB file from AFDB, instead of hardcoding the version in the URL.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `c631e2562185`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.