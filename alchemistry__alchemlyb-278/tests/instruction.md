# Issue

**Title:** Use fixture for the test

## Description

Fix #206 Merge after #254
This PR makes all the parsing for the tests done at the fixture level.
I have not touched the parsing part as the test itself is to check the parsing.
I have also not touched the ABFE part as I want to refactor the test a bit in another PR, where I might remove or change some of the tests. This PR didn't change the tests only had them load data from the fixture

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `0095be256b36`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.