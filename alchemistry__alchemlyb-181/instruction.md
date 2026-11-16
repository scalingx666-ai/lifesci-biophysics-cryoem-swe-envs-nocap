# Issue

**Title:** prepare release 0.6.0

## Description

- for #174 
- officially support Python 3.10 (keep support for Python 3.7 even though we could drop it with NEP29) 
- updated docs
- update CHANGES

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `a1c0b09dc0cc`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.