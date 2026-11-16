# Issue

**Title:** Made tests uninstallable

## Description

Based on @orbeckst's recommendation, made tests part of the package itself. Addresses #182. I tried to modify Travis CI file, but we'll see if it works.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `1737771d3773`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.