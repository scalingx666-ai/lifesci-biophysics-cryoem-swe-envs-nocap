# Issue

**Title:** Add github actions

## Description

This makes it so it compiles and runs a simple example on linux, mac (windows doesn't work with cmake yet, see: ) or every pull request or push.

To make it work I've updated Makefile to use $FC env variable, I think it is pretty much the standard. No changes to the cmake makefile. Added a folder with a simple example.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `01b9838ce142`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.