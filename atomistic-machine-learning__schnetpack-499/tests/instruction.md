# Issue

**Title:** check for legacy API-key in materials project

## Description

Raise exception if users try to use invalid API keys and direct them to the legacy materialsproject website.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `fbeb22c2a589`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.