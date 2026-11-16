# Issue

**Title:** test_topology fix

## Description

fix `test_topology_proposal/test_propose_self` so that the insulin template doesn't choose a cysteine residue (which has a disulfide) at random

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `63a03a724901`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.