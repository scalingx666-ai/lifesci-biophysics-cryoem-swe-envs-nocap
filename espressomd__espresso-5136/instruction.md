# Issue

**Title:** Fix P3M edge case in charge assignment

## Description

Description of changes:
- P3M, P3MGPU and DipolarP3M no longer trigger an assertion when particles are close to a local mesh boundary

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `08abad428c7a`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.