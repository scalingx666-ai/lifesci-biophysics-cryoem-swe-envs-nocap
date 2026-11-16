# Issue

**Title:** Fix t4 tests

## Description

#1805 introduced tests that should only run when RDKit is installed, but since some of them are slow tests, they didn't run in the normal CI until after the PR was merged (since regularly the slow tests only run in nightlies)

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `33d8496435f8`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.