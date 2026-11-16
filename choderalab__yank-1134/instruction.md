# Issue

**Title:** Update travis to test python 3.6 and 3.7

## Description

Yank, omnia, and conda-forge no longer support python 3.5.

This PR updates travis to test Python 3.6 and 3.7, pushing docs on the 3.7 build.

The Python 3.7 build is now correctly tested with the nightly builds of openmm---that had been broken previously because the wrong channel was specified.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `9c480fc5e0a6`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.