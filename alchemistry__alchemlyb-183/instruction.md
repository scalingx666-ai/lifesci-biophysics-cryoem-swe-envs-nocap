# Issue

**Title:** Implement a more robust gmx parser

## Description

Fix #171 
Fix #126

I tried to do this as a preprocessing step but I cannot get it to fix the issues that I tested in the test files, so I implement it inside the parser.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `ff491d755d88`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.