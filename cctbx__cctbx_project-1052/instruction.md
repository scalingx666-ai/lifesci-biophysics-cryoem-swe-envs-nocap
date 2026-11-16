# Issue

**Title:** Don't use cxx11

## Description

This fix:
1. Removes the `--enable_cxx11` flag
2. Ensures that `--cxxstd=c++14` is set

It follows similar patterns in the `bootstap.py` file. This is necessary to make the `alcc-recipes` deployment on Perlmutter work after the most recent compiler toolchain update (which also replaces the boost install with conda forge's boost).

Before merging, we might want to discuss if this is the best way to implement "a non-ancient c++ standard". E.g.:
1. Do we want to enable even more recent C++ versions? (E.g. let the user specify the C++ standard and just ensure that it's "at least C++14")
2. Do we want to remove the `--enable_cxx11` flag alltogether?

Anyone on Perlmutter, please give https://github.com/JBlaschke/alcc-recipes/blob/master/cctbx/setup_perlmutter_new.sh a try to see if it also works for you.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `8131662ce3b5`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.