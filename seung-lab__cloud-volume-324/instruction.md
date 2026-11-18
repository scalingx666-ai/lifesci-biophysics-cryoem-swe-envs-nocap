# Issue

**Title:** feat: add image write locking per mip level

## Description

This sets the property "locked" to true in each selected mip within info['scales']. It is then checked when calling upload or delete.

This mechanism is very vulnerable to race conditions, and is mainly intended for an additional safety mechanism to prevent accidental deletion. It's not meant to be called frequently.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `397f026175e2`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.