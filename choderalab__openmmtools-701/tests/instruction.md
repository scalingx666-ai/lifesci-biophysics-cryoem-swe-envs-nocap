# Issue

**Title:** manually casting objects to np arrays when reading checkpoint

## Description

## Description
Numba 0.57 explicitly raises an exception when trying to use `MaskedArrays` in `@jit` decorated functions. When we are reading `MultiStateSampler` attributes from serialized objects (storage) these attributes become `MaskedArray`. We are casting them to arrays here to make sure we can still use numba when mixing replicas after resuming the simulation.

Solves #700 

## Todos
- [x] Implement feature / fix bug
- [ ] Add [tests](https://github.com/choderalab/openmmtools/tree/master/openmmtools/tests)
- [ ] Update [documentation](https://github.com/choderalab/openmmtools/tree/master/docs) as needed
- [ ] Update [changelog](https://github.com/choderalab/openmmtools/blob/master/docs/releasehistory.rst) to summarize changes in behavior, enhancements, and bugfixes implemented in this PR

## Status
- [ ] Ready to go

## Changelog message
```
Support for latest numba 0.57 by deserializing attributes as numpy arrays instead of MaskedArrays.
```

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `abd4011f1c75`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.