# Issue

**Title:** ContextCache usage cleanup

## Description

## Description
Given that the `ContextCache` is a performance _trick_. It is better that it doesn't get serialized at all and that it is only used when the moves are applied or energies are computed. 

The `mcmc` module classes won't have a `context_cache` kwarg, instead the `apply` method for each will get fed the `context_cache` from the `multistatesampler` module objects.

This should tackle the memory issues when dealing with multiple replicas (  ) and also work around the cycling issue ( ). 

## Todos
- [x] Implement feature / fix bug
- [x] Add [tests](https://github.com/choderalab/openmmtools/tree/master/openmmtools/tests)
- [ ] Update [documentation](https://github.com/choderalab/openmmtools/tree/master/docs) as needed
- [ ] Update [changelog](https://github.com/choderalab/openmmtools/blob/master/docs/releasehistory.rst)Notable points that this PR has either accomplished or will accomplish.

## Status
- [x] Ready to go

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `6d6664736f41`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.