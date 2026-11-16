# Issue

**Title:** Fix: API and docs updates to allow copying/linking outside job directory

## Description

Goes with some in-development changes in CryoSPARC. Also improves API for copying files/folders. Drops `_rel` suffix from path names, since most of these can now be absolute.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `6c88a93b1214`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.