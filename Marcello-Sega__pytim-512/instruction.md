# Issue

**Title:** Handle SASA pickling failures on Python 3.14

## Description

## Summary
- unpicklable class problems surfaced using python 3.14
- switch SASA multiprocessing setup to use a multiprocessing context + use a static function to avoid the pickling issue
- catch pickling and process errors, falling back to single-core execution in unhandled cases, raising a warning

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `4707acbe7788`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.