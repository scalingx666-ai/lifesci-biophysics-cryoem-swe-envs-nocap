# Issue

**Title:** `LangevinDynamicsMove` now accepts `constraint_tolerance` parameter

## Description

## Description
`LangevinDynamicsMove` now accepts a `constraint_tolerance` parameter (defaults to the same value as the previously used `LangevinIntegrator`).

 

## Todos
- [x] Implement feature / fix bug
- [x] Add [tests](https://github.com/choderalab/openmmtools/tree/master/openmmtools/tests)
- [x] Update [documentation](https://github.com/choderalab/openmmtools/tree/master/docs) as needed
- [x] Update [changelog](https://github.com/choderalab/openmmtools/blob/master/docs/releasehistory.rst) to summarize changes in behavior, enhancements, and bugfixes implemented in this PR

## Status
- [ ] Ready to go

## Changelog
```
``LangevinDynamicsMove`` now allows ``constraint_tolerance`` parameter and public attribute, for specifying the fraction of the constrained distance within which constraints are maintained for the integrator (Refer to `Openmm's documentation <http://docs.openmm.org/latest/api-python/generated/openmm.openmm.LangevinMiddleIntegrator.html#openmm.openmm.LangevinMiddleIntegrator.setConstraintTolerance>_ for more information).
```

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `85f6220e906c`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.