# Issue

**Title:** Drop distutils usage and add Python 3.12 tests

## Description

The `distutils` module has been removed from Python 3.12 per [PEP 632](https://peps.python.org/pep-0632/).

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `81209d00817a`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.