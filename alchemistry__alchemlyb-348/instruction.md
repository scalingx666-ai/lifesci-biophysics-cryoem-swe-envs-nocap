# Issue

**Title:** Lammps Support

## Description

Resolves: Issue #349 

This module will allow for alchemlyb to be used with LAMMPS outputs. An independent package for easily generating the LAMMPS input files will shortly follow, although the functions have been designed to be flexible.

In addition to the ``extract_dHdl`` and ``extract_u_nk`` functions, additional functions, ``extract_dHdl_from_u_n`` and ``extract_u_nk_from_u_n`` have been added to allow these data frames to be generated from potential energy information that is separable from the target variable (e.g., epsilon in the LJ).

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `1aa56d73f3dd`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.