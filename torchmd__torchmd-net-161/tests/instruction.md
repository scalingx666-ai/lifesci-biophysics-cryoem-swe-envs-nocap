# Issue

**Title:** Fix deprecated usage of np.int with numpy>1.23

## Description

Tests failed for me (with the suggested environment.yml, which installed numpy=1.24) due to usage of the [deprecated numpy.int](https://numpy.org/devdocs/release/1.20.0-notes.html#using-the-aliases-of-builtin-types-like-np-int-is-deprecated).
I replaced a couple of instances of np.int by just int, as suggested in the numpy docs.

All tests pass with the numpy 1.24.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `9ab6280714aa`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.