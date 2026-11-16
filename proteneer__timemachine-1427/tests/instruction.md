# Issue

**Title:** Upgrade to Cuda 12.4.1

## Description

* Switch to new CI tags
* Upgrade to Cuda 12.4.1
* Updates docker caching to more suited to new infrastructure.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `8bf96fc2ef06`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.