# Issue

**Title:** Add exception for new pylint check

## Description

Tests on the master branch fail because of a Pylint upgrade. I'll merge this (trivial) fix when tests have passed.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `d7eccdf8db88`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.