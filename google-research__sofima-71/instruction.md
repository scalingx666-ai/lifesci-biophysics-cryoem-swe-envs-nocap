# Issue

**Title:** Remove explicit default instantiation from flow_config in favor of listing sane

## Description

Remove explicit default instantiation from flow_config in favor of listing sane
defaults in the processors themselves. These defaults may have to change in the
future to accomodate 3D flow alignment.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `64d5c7ce2c2d`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.