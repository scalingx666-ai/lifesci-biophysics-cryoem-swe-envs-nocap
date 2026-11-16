# Issue

**Title:** Version 0.9.0

## Description

- fixes for MDAnalysis 2.0.0
- switches to circleci

more in detail:

* fix missing attributes
* follow convention for numpy dtype
* update interface for query_ball_point
* fix in example calculation of profile
* small python/numpy fixes
* exclude openmm from tests for the time being
* fix precision issue in orientation test
* minor fixes

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `6d86692a48eb`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.