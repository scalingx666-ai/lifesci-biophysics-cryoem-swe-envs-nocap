# Issue

**Title:** Load tools automatically

## Description

Proposal for tool loading as in issue #68. Awaiting for opinions or changes proposals.

Even if almost complete it remains largely untested (eg. `extra` and `groups`).

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `e98bce58a367`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.