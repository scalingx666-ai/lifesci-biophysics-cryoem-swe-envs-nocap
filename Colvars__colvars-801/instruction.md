# Issue

**Title:** fix: synchronize the reduction change from NAMD

## Description

The submit of reductions has been moved to the main GlobalMaster class. See https://gitlab.com/tcbgUIUC/namd/-/merge_requests/410 for more information.

This PR also requires updating the https://github.com/Colvars/namd repo.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `20146f6de251`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.