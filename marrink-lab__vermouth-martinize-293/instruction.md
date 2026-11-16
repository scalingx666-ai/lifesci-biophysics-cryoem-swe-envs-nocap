# Issue

**Title:** Fix mistakes in martini22p and elnedyn data files

## Description

These fixes come from the integration tests branch, and fix a few mistakes in the martini22p and elnedyn data files. In particular, it moves termini to be modifications (+ associated mappings), fixes the mappings for elnedyn thr and val, and corrects a sidechain angle for elnedyn trp.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `f33eb3d3c572`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.