# Issue

**Title:** MAX_SQL_ITEMS as class variable

## Description

This converts the `max_query_size` that @sroet had introduced as an instance variable in `SQLStorageBackend` to a class variable, and adds its use in a corner case that @gyorgy-hantal came across.

It makes more sense to have this as a class variable because it is determined by the way the SQL backend (usually SQLite) was compiled on the local machine, and therefore will be constant throughout a session (for however many files the user opens). Of course, it can still be overridden on an instance-by-instance basis.

Currently WIP because I'm hoping that I can get a regression test, but I may mark it as ready to merge without such a test if it is too annoying to create. @gyorgy-hantal has confirmed that this fixed his issue.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `8292193a4456`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.