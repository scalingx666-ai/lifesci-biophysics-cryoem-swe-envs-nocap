# Issue

**Title:** Fix Github CI

## Description

Pin dependency versions and some general restructuring

Wandb server sync is currently disabled during testing, but we only push to it anyway.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `963508c7ee31`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.