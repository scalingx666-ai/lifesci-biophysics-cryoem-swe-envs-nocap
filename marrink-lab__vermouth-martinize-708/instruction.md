# Issue

**Title:** Missing dssp flag warning

## Description

Since it can be a bit dangerous to forget the dssp flag, I have added a warning  in case the dssp flag is missing. Thanks for suggesting that  @vaneerdf 

Addresses #707

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `cbb18086baa8`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.