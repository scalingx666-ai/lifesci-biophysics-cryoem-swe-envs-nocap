# Issue

**Title:** Biological assembly builder, transformation ordering fix

## Description

Fix for the issue #791, total order is now imposed.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `1beedcbc7e5d`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.