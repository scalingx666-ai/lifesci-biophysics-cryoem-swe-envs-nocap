# Issue

**Title:** Fix LOD out of range error

## Description

A small fix to produce a useful exception when lod requested is out of range on a ShardedMultiLevelPrecomputedMeshSource.

(@schlegelp on )

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `bac03bac49b3`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.