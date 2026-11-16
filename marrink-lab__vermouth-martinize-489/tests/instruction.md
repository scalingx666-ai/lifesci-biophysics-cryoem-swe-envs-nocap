# Issue

**Title:** Install dssp=3.0 during CI

## Description

The github CI runner changed to 22.04 (from 20.04), which has DSSP version 4.0 (https://packages.ubuntu.com/jammy/dssp), which doesn't work with vermouth. So try to specify a different version. If this works we can add a version check to the DSSP processor.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `e383b48bb533`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.