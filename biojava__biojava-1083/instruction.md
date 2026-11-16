# Issue

**Title:** More lenient mmCIF parsing

## Description

[ESMAtlas](https://esmatlas.com/) files miss some of the atom_site fields, which was breaking their parsing. This makes parsing more lenient, not requiring these fields if not present.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `ab17e62725ba`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.