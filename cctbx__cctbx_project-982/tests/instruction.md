# Issue

**Title:** Reduce2 updates

## Description

Makes Reduce2 use the same wrapping of Probe2 that Clashscore2 uses, which is much cleaner.

Factoring out argument construction in reduce_hydrogens.py so that it can be reused in reduce2.py.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `6073a8579546`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.