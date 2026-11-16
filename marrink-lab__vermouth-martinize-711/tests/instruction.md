# Issue

**Title:** remove -idr-tune flag.

## Description

If `-id-regions` is given, use id features automatically. This cleans things up a bit and makes sure that a small missing flag doesn't generate wrong parameters.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `8ee95b8f0102`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.