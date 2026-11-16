# Issue

**Title:** Adds support for different minimization schemes

## Description

* Starting point to explore different Minimization approaches, adds support for Scipy and FIRE. Allows users to adopt different parameters without having to adjust the details of minimization.
* Does not address minimization issues with clashy molecules, will be done in a follow up PR.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `78bab8a729be`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.