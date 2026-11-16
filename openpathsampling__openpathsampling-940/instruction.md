# Issue

**Title:** SimStore: Tags

## Description

This PR adds a `tags` table to SimStore storages. As with `netcdfplus`, the `tags` table can be used to store arbitrary data. Any JSON-serializable object, or any object with a SimStore UUID, can be saved as a named tag.

This PR also includes some additional cleanup after #929 and miscellaneous support so that SimStore can be used in the OPS CLI (see also ).

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `86eafe249e32`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.