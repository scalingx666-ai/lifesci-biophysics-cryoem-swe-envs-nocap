# Issue

**Title:** Fix H-bond detection bug/inconsistency in CHAPI

## Description

Fix inconsistency in CHAPI where donor acceptor atoms without Hs present were treated as a clash, whereas in GUI they are not.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `c7aaaa78265d`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.