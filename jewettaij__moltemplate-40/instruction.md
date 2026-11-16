# Issue

**Title:** DREIDING dihedral bug fix

## Description

Fixed an issue with DREIDING dihedral code that caused some labels to be
in the wrong atom positions (well spotted Andrew!)
This wont have impacted prior force field testing.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `c073678ce9ff`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.