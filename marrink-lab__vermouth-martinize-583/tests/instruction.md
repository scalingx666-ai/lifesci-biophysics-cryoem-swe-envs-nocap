# Issue

**Title:** Fix #558 by inverting the link logic generating charged backbone charge dummy exclusions for polarizable forcefields

## Description

Turns out those exclusions were missing from elnedyn22p completely as well...

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `ad27bdf4a713`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.