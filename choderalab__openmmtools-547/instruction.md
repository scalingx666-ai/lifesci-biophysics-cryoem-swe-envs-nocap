# Issue

**Title:** More streamlined context caches usage

## Description

## Description
Context caches for propagation and energy computations are expected to be handled only via `MultiStateSampler` instance attributes. Default behavior is to use global cache. If users want to avoid cycling they are expected to specify different context caches via the instance attributes.

Since we are not serializing context cache information, resuming simulations requires users to specify context cache attributes upon resuming.

 

## Todos
- [x] Implement feature / fix bug
- [x] Add [tests](https://github.com/choderalab/openmmtools/tree/master/openmmtools/tests)
- [x] Update [documentation](https://github.com/choderalab/openmmtools/tree/master/docs) as needed
- [x] Update [changelog](https://github.com/choderalab/openmmtools/blob/master/docs/releasehistory.rst)Notable points that this PR has either accomplished or will accomplish.

## Status
- [x] Ready to go

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `f1dc94240625`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.