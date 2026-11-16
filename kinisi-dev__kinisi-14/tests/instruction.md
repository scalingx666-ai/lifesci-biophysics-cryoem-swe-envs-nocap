# Issue

**Title:** Enable dimensionality investigations

## Description

This PR will allow #13 to be closed. It enables control over which Cartesian dimensions the MSD and therefore diffusion is calculated in. 

*Note*: the reason for the keyword argument being in the `bootstrap_params` is that this will allow users to read the data in once and run each dimension individually without reading the data in again. In future this can be exposed to the API but currently it requires constructing things yourself.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `633d77033260`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.