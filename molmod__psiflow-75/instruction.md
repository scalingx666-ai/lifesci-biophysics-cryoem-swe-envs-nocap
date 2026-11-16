# Issue

**Title:** Make client wait for socket to appear

## Description

Replaces the hardcoded `sleep 3s` between I-PI `server` and `client` startup. #74

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `2def01b55174`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.