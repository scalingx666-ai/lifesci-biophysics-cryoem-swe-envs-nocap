# Issue

**Title:** Remove things deprecated for 2.0

## Description

**ANNOUNCING THE DEV-2.0 BRANCH!**

If you have changes that must change the API, please target the `dev-2.0` branch. I'm starting that off by removing all deprecated functionality (at least, what I could find).

For most improvements and new functionality, please continue to target the default branch. Changes there will be merged into `dev-2.0` regularly. I expect that there will be several more releases of OPS (at least up to 1.6) before 2.0 is finalized.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `59760c875991`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.