# Issue

**Title:** Add MO-basis RDMs fields

## Description

Hi, @PaulWAyers @FarnazH, this is my modification to allow IOData to handle MO-basis RDMs. Thoughts?
I will then add a wrapper to PyCI/PySCF and put it in IOData/Scripts, so that we can use IOData with PyCI to run HCI computations.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `73eee89111bc`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.