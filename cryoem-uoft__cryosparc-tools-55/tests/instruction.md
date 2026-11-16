# Issue

**Title:** Fix: only ever stream bytes from Dataset.stream()

## Description

This reduces weirdness for clients that use Dataset and are expecting bytes, including command_vis

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `8a7d6d88df51`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.