# Issue

**Title:** Don't reinitialize CustomIntegrator on every step when no computations are present

## Description

.  I wouldn't expect this to come up in practice, but it should prevent a CustomIntegrator with no computation steps from leaking memory.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `2abeb8016577`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.