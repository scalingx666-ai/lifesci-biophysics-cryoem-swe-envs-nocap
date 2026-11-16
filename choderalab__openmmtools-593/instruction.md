# Issue

**Title:** Handling unsampled states in offline/realtime analysis.

## Description

## Description
This aims to refactor `MultiStateSampler._offline_analysis` to handle unsampled states when doing the mbar estimate.

In this case the `_offline_analysis` method was decoupled from the `_online_analisis` one (which is still experimental). So that it initializes the first time with zeros as guess and subsequent results are updated  using the previous calculation.

For this I had to make a new private attribute `_last_mbar_f_k_offline` which gets also written to the nc file independently of the attributes used in the `_online_analysis` method.

 

## Todos
- [x] Implement feature / fix bug
- [n/a] Add [tests](https://github.com/choderalab/openmmtools/tree/master/openmmtools/tests)
- [ ] Update [documentation](https://github.com/choderalab/openmmtools/tree/master/docs) as needed
- [x] Update [changelog](https://github.com/choderalab/openmmtools/blob/master/docs/releasehistory.rst)Notable points that this PR has either accomplished or will accomplish.

## Status
- [ ] Ready to go

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `83dd2be11cf3`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.