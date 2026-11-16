# Issue

**Title:** Fixed filtering by occupancy on ANISOU records (issue #35).

## Description

. Changed the identification of lines to 'keep' by atom unique ids, skipping `ANISOU` records when reading the file but including them on the writing/filtering step.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `f9c1a6235d55`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.