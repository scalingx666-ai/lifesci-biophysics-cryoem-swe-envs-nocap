# Issue

**Title:** Spliting the `solvate` function into two separate functions: `solvate_sol`, `solvate_ion`

## Description

Gromacs commend `gmx solvate` cannot update topology after solvating system in a mixed solvent box automatically. One needs to update the topology manually. But current `setup.solvate` function does the `solvate` as well as `genion` processes. And it causes an error when performing `genion` process. 

If we split the `solvate` function into two functions which one does `solvate`, and the other one does `genion`, one can modify the topology file before performing `genion`

Changes made in this Pull Request:

 - Spliting the `solvate` function into two separate functions: `solvate_sol`, `solvate_ion`
 - Rewrite `solvate` function

PR Checklist
------------
 - [ ] Tests?
 - [ ] Docs?
 - [ ] CHANGELOG updated?

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `61b6ec5a5b97`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.