# Issue

**Title:** Rename DualTopologyMinimization -> DualTopology

## Description

This PR:

-Deprecates the old `DualTopology` class and removes its single usage from the deprecated `md/align.py` code.
-Renames the `DualTopologyMinimization` -> `DualTopology`.

Semantically, the only key difference was that for nonbonded **intermolecular** interactions, `DualTopology` ignored the `lamb` argument when setting the `w-coords`, where as `DualTopologyMinimization` obeyed it. There's no good use-case for the former.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `caaaad2a4375`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.