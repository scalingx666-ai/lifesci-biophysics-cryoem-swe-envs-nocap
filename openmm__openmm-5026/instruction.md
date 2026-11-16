# Issue

**Title:** Fix reading of Gromacs topologies created from CHARMM force field

## Description

This is the fix for issue: 

It correctly deals with NBFIX entries and 1-4 exclusions using default ('2') combination rules.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `d2f76949b3cc`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.