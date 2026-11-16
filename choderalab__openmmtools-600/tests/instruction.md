# Issue

**Title:** Fixes #599 : Change LangevinDynamicsMove to use openmm.LangevinMiddleIntegrator instead of openmm.LangevinIntegrator

## Description

## Description
 : Changes `mcmc.LangevinDynamicsMove` to use `openmm.LangevinMiddleIntegrator` (a variant of BAOAB) instead of `openmm.LangevinIntegrator` (a variant of OBABO) to provide higher quality configuration-space samples.

## Todos
- [X] Implement feature / fix bug
- [X] Add [tests](https://github.com/choderalab/openmmtools/tree/master/openmmtools/tests)
- [X] Update [documentation](https://github.com/choderalab/openmmtools/tree/master/docs) as needed
- [X] Update [changelog](https://github.com/choderalab/openmmtools/blob/master/docs/releasehistory.rst)

## Status
- [ ] Ready to go

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `478cc7b9cbc2`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.