# Issue

**Title:** Fix *.WFN load_one() function

## Description

Fixed .WFN load one by using AIMPAC Fortran-style formatting
Added test in test_wfn.py using the cah110_sto3g_g09.wfn file from Farnaz.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `b36513d162f9`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.