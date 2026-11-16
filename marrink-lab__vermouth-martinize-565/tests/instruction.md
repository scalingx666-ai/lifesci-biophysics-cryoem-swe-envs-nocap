# Issue

**Title:** Multiple mutate fixes

## Description

re-addresses #553 in the case that mutations are given across multiple chains. This PR makes sure that in the case that multiple chains are targeted, the check for the target residue existing is not present across all chains. So warnings aren't raised when the 'missing' residue is encountered.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `dbe0d76e3f4f`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.