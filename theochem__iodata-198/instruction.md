# Issue

**Title:** mwfn load_one

## Description

Completed  load_one function support for *.mwfn file types

Separate future PR's will contain load_many, dump_one, dump_many

Files where added  in three locations:
    1) tests/data/ had the following files added for testing
        ch3_hf_sto3g_fchk_multiwfn3.7.mwfn
        ch3_rohf_sto3g_g03_fchk_multiwfn3.7.mwfn
        he_spdfgh_virtual_fchk_multiwfn3.7.,wfn
   2) tests/ had the following file added for testing:
        test_mwfn.py
    3) formats/ had the following file added for testing:
        mwfn.py

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `ced78e6bd536`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.