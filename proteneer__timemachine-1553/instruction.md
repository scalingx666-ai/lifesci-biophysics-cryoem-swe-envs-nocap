# Issue

**Title:** Expand REST selection heuristics

## Description

This PR will expand the REST region to "complete-rings", including its 1-connected neighbors (`H-, F-, Cl-, Br-`, etc) and 1-2 connected neighbors (`H-O-`, `N#C-`), etc.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `9b6d620f0e9f`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.