# Issue

**Title:** Fixed ReadSequence in genpoly_lt.py for generating 1 cut polymer with…

## Description

… sequence.txt.  This occurs when using sequence.txt file for polymer with no cuts set.  Fixed by using same logic as ReadCoords.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `b4f1c96f3843`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.