# Issue

**Title:** Let UUID determine equality for ensembles

## Description

Long ago, we tried to use the string representations of ensembles as the comparison for equality. This was a decent idea, although there are some issues with the ways we handle equality and hashing.

I recently came across a weird edge case where a periodic volume with an edge exactly at -pi was serialized in 2 different ways: one with the edge at -pi and one with the edge at +pi. I'd still like to look into what happened to make that happen, but this would fix the most obvious effect (which is that the resulting ensembles weren't equal). In any case, this is a good shortcut: the same UUID indicates the same object, assuming we can trust the meaning of UUID.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `40625306d8ad`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.