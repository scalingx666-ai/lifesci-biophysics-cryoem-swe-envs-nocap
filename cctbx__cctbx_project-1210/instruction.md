# Issue

**Title:** Add a Hessian runtype to xtb_manager

## Description

run_hessian runs xtb --hess at a given geometry and returns the path to the Cartesian Hessian file, removing a stale one first and requiring normal termination. get_cmd gains a hessian runtype, get_xyz_lines an optional geometry override, and run_cmd a hessian pass-through.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `dc361454263c`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.