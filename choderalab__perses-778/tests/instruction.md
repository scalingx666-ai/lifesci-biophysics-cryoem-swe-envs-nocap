# Issue

**Title:** add allow_undefined_stereo option for SDF files with undefined stereocenters

## Description

This is directly related to #673. 

In this PR I've added an option to set `allow_undefined_stereo` when creating an `OEMol` object using `createOEMolFromSDF` in `relative_point_mutation_setup.py`.

@schallerdavid and I are working with sets of ligands that contain undefined stereocenters after preparation with OpenEye (this has been reported to them and they are working on a fix). Until this is solved (or we move to `openforcefield`, as discussed in #657) we need an option to relax the requirement that all stereocenters need to be defined.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `697c008ce2d2`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.