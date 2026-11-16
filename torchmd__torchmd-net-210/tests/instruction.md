# Issue

**Title:** Updating to Lightning 2.0

## Description

This is an effort to update the pytorch lightning dependency ( )
A lot has changed since the LNNP module was written (current version is 1.6.4) and I expect this update would increase compatibility with some functionalities we seek (AKA torch compile/torchscript training).

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `dca66796d006`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.