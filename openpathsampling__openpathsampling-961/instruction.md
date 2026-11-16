# Issue

**Title:** Fix simtk JSON storage in SimStore

## Description

Switch to using `is_storage_iterable` everywhere.

This fixes a problem that `simtk.unit.Quantity` objects couldn't be directly tagged (or, probably, otherwised saved as part of simulation objects).

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `dc4c34691473`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.