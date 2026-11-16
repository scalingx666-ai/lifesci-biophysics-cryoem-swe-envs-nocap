# Issue

**Title:** Adds missing installation target directory to CMake compile script

## Description

In this PR, the missing target directory is added to the CMake compile script. Then, the packmol binary is installed under bin/ of the packmol root directory.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `9ddec8c911a7`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.