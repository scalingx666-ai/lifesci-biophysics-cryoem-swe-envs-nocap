# Issue

**Title:** removed warnings from: crystd, eds, fisst, funnel, logmdf, sasa, sizeshape, ves

## Description

these were only issue with order of initialisation and unused variables

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `81cdca45f9e3`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.