# Issue

**Title:** Checked if the recodings are empty

## Description

I inserted a check if the recording DataFrame is empty, if so I raised an value error. Nevertheless, I use a WSL and at some point the tests got stuck, at about 80% of completion, and  the WSL lost connection.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `61c5c276c867`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.