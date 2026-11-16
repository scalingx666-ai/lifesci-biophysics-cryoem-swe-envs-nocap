# Issue

**Title:** Fix #946 Parse PDB KEWODS record

## Description

They are parsed from PDB and added to Structure.
Not parsed from mmCIF yet.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `37991a7bfe41`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.