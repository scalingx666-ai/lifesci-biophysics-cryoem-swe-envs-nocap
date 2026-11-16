# Issue

**Title:** Make non-MTS colvars always awake

## Description

Simple fix for Issue #865

In hindsight, this is a very logical part of implementing the MTS system.

Edit to clarify: this increases the refcount of `f_cv_active` by 1, making sure that the colvar stands on its own even when it is not referenced by a bias.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `dfe58a191408`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.