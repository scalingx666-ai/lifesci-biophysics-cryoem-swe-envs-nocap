# Issue

**Title:** flipped x and z axes in marching_cubes output for consistency

## Description

The surface data outputted by Writevtk.surface contained normal vectors whose x and z axes were flipped relative to the ordering used for the surface vertices, leading to normals appearing not to be normal to the surface in visualization.

The underlying cause of this problem seems to lie in the marching_cubes_lewiner function from skimage, in which the axes are flipped for both vertices and normals because (according to a comment in the code) "skimage uses zyx ordering"...

Fixed by flipping the axes back after the call to marching_cubes.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `4f7e92235262`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.