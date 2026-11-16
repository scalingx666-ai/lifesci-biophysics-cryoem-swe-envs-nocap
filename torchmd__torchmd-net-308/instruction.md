# Issue

**Title:** Added support for ensemble models

## Description

Works fine but I don't like so much that the same function can return a model with 4 outputs (energy/force mean/std) or with 2 outputs (just energy, force) depending on if you pass a single file or multiple.
Maybe it could become an argument to load_model to disable the returning of the std?

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `9546e88fba0c`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.