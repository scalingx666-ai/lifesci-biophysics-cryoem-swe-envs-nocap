# Issue

**Title:** Bump to Autorelease 0.3.2

## Description

Autorelease 0.3.2 was released back in August. The main improvements are in blocking Autorelease workflows from running on forks () and in better information on the vendored files (to ensure that the status as vendored from Autorelease is more clear).

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `ddfe508bbe30`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.