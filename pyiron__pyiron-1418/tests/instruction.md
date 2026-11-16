# Issue

**Title:** Update postBuild - for pyiron in binder

## Description

The lines for building nglview with jupyter are now deleted, as recommended by @muh-hassani in issue #1417.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `4e6360ec8426`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.