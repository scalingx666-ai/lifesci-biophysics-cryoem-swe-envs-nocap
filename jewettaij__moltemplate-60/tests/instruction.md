# Issue

**Title:** Fixing attribute double initialization.

## Description

self.coords_multi the value is already properly initialized in the constructor, its use in this part seems to prevent manual assignment by the client of the class.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `fb8eeb572393`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.