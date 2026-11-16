# Issue

**Title:** Match Niggli iteration-limit exception message

## Description

The Niggli reduction algorithm was ported from python to c++ in PR
1152. When the iteration-limit was exceeded, the python code exception string was "Krivy-Gruber iteration limit exceeded" and the c++ port is "Niggli reduction iteration limit exceeded". This PR changes the c++ exception to match.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `7cfb03df5ebf`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.