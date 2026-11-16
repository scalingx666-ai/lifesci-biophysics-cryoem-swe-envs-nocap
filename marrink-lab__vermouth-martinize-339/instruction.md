# Issue

**Title:** Fix 317 by ensuring repair_graph prefers matching non-modification atoms

## Description

This is combined by a cleanup of _patch_modification by relabeling all nodes to integers, and unifying the treatment of anchor and non-anchor nodes.

In ISMAGS, when mapping a subgraph node to a candidate, sort the candidates first so that the lowest is taken preferentially.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `7c8675d51512`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.