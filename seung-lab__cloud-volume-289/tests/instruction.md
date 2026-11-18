# Issue

**Title:** fix: accurate mip level and voxel offset accomodation for vertex deduplication

## Description

We can also extract mip from the mesh path since Igneous has
been using the directory name to store the mip level used for
meshing.

e.g. mesh_mip_2_err_40

err_40 refers to allowing up to 40nm of displacement during
simplificiation.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `0d8b307d35fe`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.