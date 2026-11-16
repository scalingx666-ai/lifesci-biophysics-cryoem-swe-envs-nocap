# Issue

**Title:** Adds support for multiple bonded forces in OpenMM Systems

## Description

* More defensive, as duplicate forces can produce incomplete systems
* Useful if needed to build custom OpenMM systems

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `bb38a36cbbc0`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.