# Issue

**Title:** Minor changes to the Moltemplate main script.

## Description

The option -overlay-all has been redefined to have a consistent behaviour on different OSs. Cosmetic changes to the cleaning of processed files: removing head tab spaces.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `2b5c9a7626ae`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.