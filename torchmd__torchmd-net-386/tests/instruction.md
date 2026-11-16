# Issue

**Title:** fix static shapes in coulomb output model

## Description

the -1 edges were not handled properly in the coulomb output module. Sometimes it seems they get mapped to the last element, the same as doing `tensor[-1]` but other times they error out as I would expect: 
We now have correct behavior where they get set to the index of the extra dummy atom.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `df4cc6af6a3a`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.