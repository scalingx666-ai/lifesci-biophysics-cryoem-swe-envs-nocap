# Issue

**Title:** Force statistical inefficiency computation to use self.max_n_iterations

## Description

## Description
I noticed that in `MultiStateAnalyzer.generate_mixing_statistics()`, `self.max_n_iterations` is used to generate the transition matrix and compute the perron eigenvalue, but it's not used to compute the statistical inefficiency. This PR supplies `self.max_n_iterations` to the statistical inefficiency computation, so that the whole function is consistent (in terms of iterations used) in the mixing statistics it outputs.

## Todos
- [x] Implement feature / fix bug
- [ ] Add [tests](https://github.com/choderalab/openmmtools/tree/master/openmmtools/tests)
- [ ] Update [documentation](https://github.com/choderalab/openmmtools/tree/master/docs) as needed
- [ ] Update [changelog](https://github.com/choderalab/openmmtools/blob/master/docs/releasehistory.rst)Notable points that this PR has either accomplished or will accomplish.

## Status
- [ ] Ready to go

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `870d81ab5a75`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.