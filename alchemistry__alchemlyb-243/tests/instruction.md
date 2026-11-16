# Issue

**Title:** Add extract description in docs

## Description

Just a couple of things that I forgot about:
- added the description of `extract` to the main `parsers` page in the documentation
- the test in test_namd.py now properly tests if 'dHdl' is not set when calling `extract`

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `48a719b03521`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.