# Issue

**Title:** amber mbar parser

## Description

This pull request adds support for amber mbar parsing for amber .out  files by using the [extract_u_nk](https://github.com/alchemistry/alchemlyb/blob/amber-mbar-parse/src/alchemlyb/parsing/amber.py#L250) function, which I wrote to primarily mimic the output format of the [extract_u_nk](https://github.com/alchemistry/alchemlyb/blob/amber-mbar-parse/src/alchemlyb/parsing/gmx.py#L15) gromacs function. 

A [test_u_nk](https://github.com/alchemistry/alchemlyb/blob/amber-mbar-parse/src/alchemlyb/tests/parsing/test_amber.py#L35) function was also added to [test_amber.py](https://github.com/alchemistry/alchemlyb/blob/amber-mbar-parse/src/alchemlyb/tests/parsing/test_amber.py). I am now working on adding a minimal test example to [alchemtest](https://github.com/alchemistry/alchemtest).

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `a1a3e5fec26c`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.