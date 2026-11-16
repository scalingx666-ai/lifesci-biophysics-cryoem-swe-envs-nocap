# Issue

**Title:** SimStore: Periodic CVs

## Description

This PR adds information about periodicity to SimStore CVs. It reserves the kwargs `period_min` and `period_max`.

This isn't currently used anywhere in the core of OPS, but might be used in the future to combine `PeriodicCVDefinedVolume` and `CVDefinedVolume` (probably a good option for OPS 2.0).

It will be used in the CLI for simulation setup.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `e48262e9726b`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.