# Issue

**Title:** do not sort when Go

## Description

Do not sort atoms when a Go model is requested. This is for sure a quick patch but seeing that the Go model makes pretty strict assumptions about what order beads and Go sites can be in, I think it will be not too much of a problem.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `fb4fc360cb86`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.