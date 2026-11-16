# Issue

**Title:** Improve the logic behind the MakeBonds processor. Add tests

## Description

This will make the make_bonds function not add bonds between nodes that already have edges, for example from CONECT records. This means that if both node1 and node2 have edges, make_bonds will not add one between node1 and node2, even if they are at the correct distance.
In addition, there's some of bug fixes that were previously hidden.

This is a start of fixing #202.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `7d522a440c82`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.