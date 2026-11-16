# Issue

**Title:** Add charmm data for NCAP and CCAP mappings

## Description

Fixes the other half of #472, together with #476.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `29bc3d5d98fb`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.