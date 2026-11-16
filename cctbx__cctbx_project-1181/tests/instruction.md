# Issue

**Title:** Reduce2 opt only

## Description

Adds a third 'optimize' option to the available approaches for reduce2. This neither adds nor removes hydrogens, but assumes all (non-water) hydrogens are on the model and optimizes them.

Tested by running mmtx.debugging.hydrogenate on a model followed by mmtbx.reduce2 approach=optimize; got the same result as running mmtbx.reduce2 approach=add on the original model.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `8ea4908975e7`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.