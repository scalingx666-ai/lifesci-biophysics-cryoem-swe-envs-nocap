# Issue

**Title:** Added option for shell in Popen

## Description

Needed to use the `shell = True` argument for my mdrun command. No easy way to do this currently, so added it as a class attribute. Really simple change.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `c2cc490551fd`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.