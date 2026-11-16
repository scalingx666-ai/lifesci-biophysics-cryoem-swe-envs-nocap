# Issue

**Title:** chore: separate network.py and network.js

## Description

This PR moves the embedded JavaScript logic from `plotting/network.py` into a standalone file `plotting/network.js`.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `f2f3a10bc7e5`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.