# Issue

**Title:** Bug-Fix: Fallback to empty dict when `metrics=None` in `ModelOutput`

## Description

Right now the `ModelOutput` class fails to initiate when the `metrics` argument is unset even though it is labeled as optional.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `09b2551615c7`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.