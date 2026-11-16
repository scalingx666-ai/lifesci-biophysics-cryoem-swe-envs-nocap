# Issue

**Title:** Learnable Atomref

## Description

I added a new prior, LearnableAtomref, which makes atomic reference values fully learned.

It is implemented in terms of Atomref, being more of a convenience class to make the feature more evident.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `0844242c6d50`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.