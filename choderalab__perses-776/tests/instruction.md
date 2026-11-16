# Issue

**Title:** fix nightly builds

## Description

Namespace changes are coming in openmm, they are backwards compatible if we modify our import slightly.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `b06ff87cc065`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.