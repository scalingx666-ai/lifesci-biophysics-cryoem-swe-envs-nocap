# Issue

**Title:** Fix mdtraj

## Description

Patches for MDtraj and OpenMM were somehow not imported correctly anymore. This patch fixes the issue and adds an MDtraj test for Travis.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `fef974222631`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.