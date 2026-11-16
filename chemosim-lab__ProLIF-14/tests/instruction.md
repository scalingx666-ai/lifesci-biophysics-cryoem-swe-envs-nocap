# Issue

**Title:** Fix issue #13: residues with resnumber=0

## Description

Fixes issue #13 

Changed:
* Default residue name and number: `UNK` and `0` are now the default values if `None` or `''` is given

Fixed:
* Residues with a resnumber of `0` are not converted to `None` anymore

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `3286c4d3deca`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.