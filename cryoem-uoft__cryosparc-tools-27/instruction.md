# Issue

**Title:** Fix: auto-exclude slots with missing metafile and better error message when output metafile is missing

## Description

Example output:

> ValueError: Cannot load output particles_class_0 slot pick_stats because output does not have an associated dataset file. Please exclude this output from the requested slots.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `2e6634d237ca`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.