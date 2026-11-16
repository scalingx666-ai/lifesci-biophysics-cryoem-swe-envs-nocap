# Issue

**Title:** Ensure unique slurm output files

## Description

Without this calphy runs starting in the same directory, e.g. multiple calculations in one input yaml, overwrite each others slurm logs.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `c2e18bf6f2bc`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.