# Issue

**Title:** Listconversion for MDP files

## Description

Lists in MDP files will be converted into numpy arrays if autoconvert=True is set.
This enhancement can be useful for changing list parameters in the mdp file i.e. lambda states.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `80573e4333c4`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.