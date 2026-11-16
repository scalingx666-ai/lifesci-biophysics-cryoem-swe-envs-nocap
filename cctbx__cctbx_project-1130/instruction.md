# Issue

**Title:** Small cell cake plot modifications

## Description

The existing cake plot work flow was no longer working. This PR is a replacement. Key differences:

- Previously was a two-step process (cake_plot_prep.py then cake_plot.py). It is now a one-step process.
- MPI functionality has been removed.
- Standard DIALS experiment and reflection inputs using ArguementParser.
- Plot is colored by panel.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `6efbff4ed99a`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.