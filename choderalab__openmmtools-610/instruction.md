# Issue

**Title:** Fix equilibration detection

## Description

## Description

Changes: 
- Do not allow user to specify the `statistical_inefficiency` without `n_equilibration_iterations`
- When `n_equilibration_iterations` is specified and `statistical_inefficiency` is not, make sure that the final `n_equilibration_iterations` includes the t0 selected from the time origin analysis (aka `i_t[i_max]` [here](https://github.com/choderalab/openmmtools/blob/main/openmmtools/multistate/multistateanalyzer.py#L2055))
- Always discard the first time origin in `_get_equilibration_data_per_sample()`
- Add API point for specifying `max_subset` when creating the a `MultiStateSamplerAnalyzer` 

For more context on why these changes are necessary, see 

Fixes 

## Todos
- [x] Implement feature / fix bug
- [x] Add [tests](https://github.com/choderalab/openmmtools/tree/master/openmmtools/tests)
- [ ] Update [documentation](https://github.com/choderalab/openmmtools/tree/master/docs) as needed
- [x] Update [changelog](https://github.com/choderalab/openmmtools/blob/master/docs/releasehistory.rst) to summarize changes in behavior, enhancements, and bugfixes implemented in this PR

## Status
- [x] Ready to go

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `cec8c3d6627d`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.