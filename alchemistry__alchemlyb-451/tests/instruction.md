# Issue

**Title:** Add ignore_equilibration flag to NAMD parser

## Description

This is useful because equilibrium detection is often run after the fact anyway.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `0b3d44fb9857`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.