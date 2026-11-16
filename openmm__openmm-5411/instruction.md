# Issue

**Title:** Clone CustomNonbondedForce with XmlSerializer::clone()

## Description

.  We really should get rid of the copy constructor altogether, but that's a bigger change, so I'll save it for another PR.  I'm including just the minimal fix here so we can include it in 8.6.1.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `c381ec78eb60`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.