# Issue

**Title:** Fix bug experiment option and new status command

## Description

@Lnaden I ended up implementing the new `status` command too for the release because it would be incredibly useful to have it for the benchmark calculations.

- Fixed bug where options in experiment section were not coerced.
- It is possible to save dictionaries on the netcdf file in nested format now. This makes it possible to retrieve dictionary keywords from disk without loading the entire dictionary. We could use this in the future to retrieve the `temperature` very quickly from the thermodynamic state dictionary and speed up the analysis!
- Fix #852: faster restart and fix  `status` command to print information about all combinatorial experiments.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `9f7e25edb079`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.