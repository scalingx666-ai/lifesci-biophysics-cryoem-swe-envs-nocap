# Issue

**Title:** Add library functions for bootstrapping

## Description

This PR pulls out some of the common bootstrapping code into a new submodule, `perses.anaylsis.resample`.

Also includes various fixes:

- Typo in standard error of sum formula: 0860cd5

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `e083bd378c6a`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.