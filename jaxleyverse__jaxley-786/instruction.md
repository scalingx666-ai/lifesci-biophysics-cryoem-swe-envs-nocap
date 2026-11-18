# Issue

**Title:** Dataclamp checkpointing

## Description

As described in issue #785, fixes checkpointing for networks with clamped variables of different sizes.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `02fe2dd0a1ad`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.