# Issue

**Title:** POM Cleanup: Add Maven descriptors & remove mention of Travis CI

## Description

- need Maven descriptors for `pom.properties` files when using BioJava as dependency
- this ensures that they are added to the artifacts

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `a83058a490fc`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.