# Issue

**Title:** Fix SimStore problems with scalarize singletons in numpy 2

## Description

This is similar to one of the things we fixed in #1178 for array scalar conversion -- not sure why the tests for this one only started failing recently, but this will solve it.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `e6f8b8942027`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.