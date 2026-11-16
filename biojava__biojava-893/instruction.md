# Issue

**Title:** Added github actions to verify the build on each push

## Description

On each push mvn verify will be executed on Ubuntu 18.04 and the latest Mac OS X using JDK 11

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `0040dd8456c8`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.