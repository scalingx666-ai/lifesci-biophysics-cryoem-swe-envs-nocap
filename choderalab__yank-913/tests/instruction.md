# Issue

**Title:** Remove dependencies from setup.py - cut down meta.yaml build

## Description

This speeds up conda build times by reducing the number of packages which have to be downloaded.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `9172eb9bf033`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.