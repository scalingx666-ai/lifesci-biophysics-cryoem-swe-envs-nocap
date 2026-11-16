# Issue

**Title:** Switch to individual views for particle properties in place of aosoa

## Description

* Code change is minial, as slices and views can be accessed in the same way.
* I held off on renaming the AosoaPack to limit merge conflicts with other work. Should eb renamed to sth like PartPropArrays or similar
* I added id_to_index to the AosoaPack, as it will be needed when we parallelize the bond loop.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `691ad682e9b4`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.