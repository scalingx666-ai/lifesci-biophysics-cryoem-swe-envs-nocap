# Issue

**Title:** Feat/lazy load imports

## Description

Moving all of our `openeye` imports inside the methods/functions that need them. I didn't do this to the examples folder but everything else has been moved except for `perses/rjmc/topology_proposal.py` which uses `openeye` extensively for some global constants. I will have to think about the best way to edit that file, thoughts @jaimergp @jchodera for that one?

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `c5ea391a7919`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.