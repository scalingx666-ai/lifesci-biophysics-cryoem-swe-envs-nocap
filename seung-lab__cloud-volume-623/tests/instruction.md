# Issue

**Title:** Fix Callable type annotation

## Description

On Python versions <3.9, the first argument must be a list. Later versions of Python automatically coerce the arguments to work, but this maintains compatibility with old versions of python.

 .

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `afc285a987ee`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.