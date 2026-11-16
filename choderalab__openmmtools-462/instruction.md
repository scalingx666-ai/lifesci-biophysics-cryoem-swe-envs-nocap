# Issue

**Title:** Fix restrained atom indices

## Description

This is a port of a fix that was implemented by @SimonBoothroyd in choderalab/yank#1167 to fix choderalab/yank#1166 before the `multistate` subpackage was moved from YANK to OpenMMTools. See those issues for the details.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `82f14f00c97b`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.