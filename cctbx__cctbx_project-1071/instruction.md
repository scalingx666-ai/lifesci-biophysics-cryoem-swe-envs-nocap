# Issue

**Title:** Histidine h bonds fix

## Description

Fix for issue  where the Histidine ring Nitrogens were not being marked as acceptors in Probe2 or Reduce2.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `46c5f7804ad3`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.