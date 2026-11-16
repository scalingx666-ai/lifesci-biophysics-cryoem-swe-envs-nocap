# Issue

**Title:** Fixes on optimization of subunit clusterer for quaternary sym detection of PDB deposited structures

## Description

A  

This adds tests and fixes a few problems in the implementation (wasn't updating equivalent residues indices for chains of same entity). Also added docs and a new utility method in StructureTools.

In any case as of this PR I can't reproduce anymore the dramatic difference in runtime mentioned in #857 . One of the new (ignored) tests in this PR is to demonstrate the runtime difference, but it's only ~ 5x

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `5840f24a7986`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.