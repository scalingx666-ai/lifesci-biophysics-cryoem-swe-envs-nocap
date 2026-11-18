# Issue

**Title:** fix: missing import for http interface

## Description

This PR fixes a missing import for brotli support on the http interface.

Also adds a test depending on neurodata open data bucket (needed to be a brotli dataset).

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `a8042e09356c`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.