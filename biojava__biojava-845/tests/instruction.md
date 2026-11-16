# Issue

**Title:** Fixing issue with MTRIXn record parsing and adding test for it

## Description

Making PDB parser not as strict and allowing for no padding at end of MTRIXn records

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `5d79d908d926`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.