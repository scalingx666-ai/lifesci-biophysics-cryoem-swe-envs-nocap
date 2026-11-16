# Issue

**Title:** Default the plotting unit to None

## Description

Fix #247
Default the unit in `plot_dF_state` , `plot_convergence` and `plot_ti_dhdl` to None.
Remove the array input in `plot_convergence`.

I have fixed the issue here @DrDomenicoMarson

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `316f2286b481`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.