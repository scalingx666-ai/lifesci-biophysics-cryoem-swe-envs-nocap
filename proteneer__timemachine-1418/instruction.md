# Issue

**Title:** Bugfix for improper parameterization and canonicalization.

## Description

This PR fixes two issues:

1) A fundamental mis-parameterization on the handler level of an improper torsion where `[i,c,k,l]` was parameterized into `[c,i,k,l]`. 
2) As a result, we need to re-adjust the canonicalization algorithm we use for impropers during `setup_intermediate_state()`

Many thanks to @mcwitt  for producing a simple repro.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `be34fbf458b5`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.