# Issue

**Title:** Test and fix for BioJava #836 to parse and save all OBO synonyms

## Description

Added a simple test to check a term with more than one synonym.
Solution is a modified implementation of Synonym.COMPARATOR.compare() that incorporates all fields of the class, case-insensitive.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `0e86964fa9b6`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.