# Issue

**Title:** Add PeriodicNonequilibriumIntegrator

## Description

## Description
This PR adds a `PeriodicNonequilibriumIntegrator`, a simple extension of `AlchemicalNonequilibriumLangevinIntegrator` that supports periodic alchemical protocols.

@dominicrufa : This is heavily based on your `Loopy` and bidirectional integrators, but since this is simpler and more compatible with the current Folding@home infrastructure, I'm going to merge this first and then return to #464 in a day or two.

## Todos
- [x] Implement feature / fix bug
- [x] Add [tests](https://github.com/choderalab/openmmtools/tree/master/openmmtools/tests)
- [x] Update [documentation](https://github.com/choderalab/openmmtools/tree/master/docs) as needed
- [x] Update [changelog](https://github.com/choderalab/openmmtools/blob/master/docs/releasehistory.rst)Notable points that this PR has either accomplished or will accomplish.

## Status
- [x] Ready to go

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `506c35437cf8`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.