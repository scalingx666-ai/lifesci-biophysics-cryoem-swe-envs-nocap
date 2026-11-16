# Issue

**Title:** Make amber parser compatible with python 3.7

## Description

As explained in  the amber parser does not work in Python 3.7.

These changes should fix this.

See https://www.python.org/dev/peps/pep-0479/#sub-proposal-decorator-to-explicitly-request-current-behaviour

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `1813920aa326`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.