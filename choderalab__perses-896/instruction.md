# Issue

**Title:** Only support openmm 7.6

## Description

This adjusts our CI to only test 7.6 + nightly, sets the min version of openmm to 7.6 and removes and only env file.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `741dd72c25f6`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.