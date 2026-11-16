# Issue

**Title:** Add SamplerState interface to OpenMM CustomCVForce

## Description

This PR gives the `SamplerState` a regular way to access the CustomCVForce variables and values which may appear in a `Context`.

 - [x] Implement feature / fix bug
 - [x] Add [tests](https://github.com/choderalab/openmmtools/tree/master/openmmtools/tests)
 - [x] Update [documentation](https://github.com/choderalab/openmmtools/tree/master/docs) as needed
 - [x] Update [changelog](https://github.com/choderalab/openmmtools/blob/master/docs/releasehistory.rst)

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `67d09f11862e`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.