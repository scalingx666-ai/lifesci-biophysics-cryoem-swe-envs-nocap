# Issue

**Title:** Update MANIFEST.in

## Description

After the changes proposed by #32 and #33, we deleted the `requirements.txt` file. Also, the README.md is included by default in the source distribution. This PR aims to cleanup the `MANIFEST.in` file.

@stefdoerr Request for Review

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `c42e1025621e`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.