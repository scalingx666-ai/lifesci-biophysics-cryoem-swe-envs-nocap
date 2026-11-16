# Issue

**Title:** Remove pypi support for python versions not allowed by conda

## Description

Synchronizes the pypi representation of pyiron with the hard limit of python >3.8 expressed on [the conda forge feedstock](https://github.com/conda-forge/pyiron-feedstock/blob/c954babdba3bcd313738db2feeae0de6412a9123/recipe/meta.yaml#L24)

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `fa61d517f2ec`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.