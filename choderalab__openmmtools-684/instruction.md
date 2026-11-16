# Issue

**Title:** Fix NaN-catching text in MultiStateSampler

## Description

## Description

We're getting errors in OpenFE during minimization of the multistate sampler. This appears to be because a handler for NaN in the FIRE minimizer was not updated to match OpenMM's changed error message. (I note that other lines seem to have updated). See  and following.

## Todos
- [x] Implement feature / fix bug
- [ ] Add [tests](https://github.com/choderalab/openmmtools/tree/master/openmmtools/tests)
- [ ] Update [documentation](https://github.com/choderalab/openmmtools/tree/master/docs) as needed
- [ ] Update [changelog](https://github.com/choderalab/openmmtools/blob/master/docs/releasehistory.rst) to summarize changes in behavior, enhancements, and bugfixes implemented in this PR

## Status
- [ ] Ready to go

## Changelog message
```
Fix NaN-catching text in MultiStateSampler
```

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `47807bba462a`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.