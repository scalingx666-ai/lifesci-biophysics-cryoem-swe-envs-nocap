# Issue

**Title:** fix: use literal strings for strings containing invalid escape backslashes

## Description

fixes 

It turns out that most of the occurrences mentioned in  were already fixed, these are just some remaining ones.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `e8fe4dc92fba`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.