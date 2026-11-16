# Issue

**Title:** LigNetwork & to_networkx improvements

## Description

Repeats #363 which seems to have been lost to the abyss of git rebase

- Migrate LigNetwork to use `to_networkx` export instead of dataframe as underlying data model.
- slight refactor of the `to_networkx` code to reduce duplication and fix linting / typing issues
- change of default values and algorithm to fix the "residue stuck inside a ring" issue

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `07a52759db25`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.