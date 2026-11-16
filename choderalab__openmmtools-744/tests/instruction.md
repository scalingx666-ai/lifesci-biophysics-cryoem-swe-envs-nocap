# Issue

**Title:** Use OMSF action for AWS self-hosted runner

## Description

This migrates us to using the new OMSF action for setting up a self-hosted runner. Pretty much the same setup as last time but maintained by OMSF infra team and isn't tied to one cloud provider.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `cbff4c8391f5`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.