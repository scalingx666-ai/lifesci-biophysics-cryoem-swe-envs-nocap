# Issue

**Title:** Replace cython-accelerated all-swap replica mixing with numba implementation

## Description

## Description
This PR replaces the [`cython`](https://cython.org/) accelerated `all-swap` replica mixing scheme with a [`numba`](https://numba.pydata.org/) implementation.

The `cython` implementation not only caused installation issues, but is also suspected to potentially be causing the temperature issues in #480 and the MPI bug in #449 (but not yet proven to).

This implementation validates in a [test notebook](https://gist.github.com/jchodera/87149ea23530204d5b20913baa420ee3), and appears to be significantly faster:
* Python: 15 s
* cython: 4.1 s
* numba: 0.03 s

## Todos
- [x] Implement feature / fix bug
- [x] Add [tests](https://github.com/choderalab/openmmtools/tree/master/openmmtools/tests)
- [x] Update [documentation](https://github.com/choderalab/openmmtools/tree/master/docs) as needed
- [x] Update [changelog](https://github.com/choderalab/openmmtools/blob/master/docs/releasehistory.rst)Notable points that this PR has either accomplished or will accomplish.

## Status
- [ ] Ready to go

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `483765d3b873`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.