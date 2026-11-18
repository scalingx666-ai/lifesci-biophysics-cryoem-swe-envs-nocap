# Issue

**Title:** BREAKING: Drop official support for Python 2.7

## Description

Retain backwards compatibility using the python_requires flag.

See https://github.com/seung-lab/cloud-volume/wiki/Policy

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `745878782fb4`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.