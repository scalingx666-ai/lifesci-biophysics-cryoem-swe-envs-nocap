# Issue

**Title:** Fix memory error in PythonForce

## Description

PythonForce didn't prevent its computation function from being freed.  This could lead to various confusing errors.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `e688b86e2047`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.