# Issue

**Title:** Fix ArrayIndexOutOfBoundsException and file validation in CathInstallation

## Description

Even after the CATH url was fixed there were still some errors in the integration test. This should fix them.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `9cd191c4dbd3`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.