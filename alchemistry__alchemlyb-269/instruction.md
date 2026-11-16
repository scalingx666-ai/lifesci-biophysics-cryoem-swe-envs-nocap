# Issue

**Title:** test on python 3.11

## Description

- close #267
- update setup.py metadata
- add Python 3.11 to test matrix
- only test lowest (3.8) and highest (3.11) python for the expensive Windows and macOS runners to reduce CI load and wait times.
- use Python 3.10 for deploy environment

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `6b51c81903b5`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.