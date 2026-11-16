# Issue

**Title:** Prevent non-string names in StorableNamedObject

## Description

Objects in OPS can be named. These names should always be strings. This PR causes a `TypeError` to be raised if a non-string is assigned as the `name` of a `StorableNamedObject`.

Some context in discussion of

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `d2488988059f`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.