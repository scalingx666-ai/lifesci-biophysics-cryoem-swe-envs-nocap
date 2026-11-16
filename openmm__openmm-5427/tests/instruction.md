# Issue

**Title:** Avoid cloning CustomNonbondedForce

## Description

This makes `CustomNonbondedForce::updateParametersInContext()` faster by avoiding the need to create a copy of the force.  See

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `b93b58e041ca`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.