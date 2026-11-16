# Issue

**Title:** Refactor the subsampling module to unify the behaviour of equilibrium_detection and statistical_inefficiency

## Description

- Add the keyword `remove_burnin` to the high-level function `decorrelate_u_nk` and `decorrelate_dhdl` to use `equilibrium_detection` instead of `statistical_inefficiency`.
- Extract the function `u_nk2series` and `dhdl2series` to make series from dataframe
- Extract the function `_drop_duplicates`, `_sort_by_time` and `_prepare_input` to clean the data before passing to `equilibrium_detection` and `statistical_inefficiency`
- Unify the behaviour of `equilibrium_detection` and `statistical_inefficiency` in terms of preprocessing and checking the data.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `69dacd6558df`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.