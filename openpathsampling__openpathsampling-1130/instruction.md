# Issue

**Title:** Updates for SQLAlchemy 2.0

## Description

SQLAlchemy 2.0 has been released, and SimStore tests are failing now. There were warnings in 1.4 that help migrate to the 2.0 API. This implements those changes.

Still have a couple places where I want to manually check the SQL output to ensure that this is the same, but this PR will fix current errors.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `7d077710a062`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.