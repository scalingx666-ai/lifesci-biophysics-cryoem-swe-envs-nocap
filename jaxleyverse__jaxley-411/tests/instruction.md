# Issue

**Title:** fix  equidistant interpolation in xyz computation

## Description

the current implementation of `compute_xyz` does not correctly compute the locations of the xyz coordinates, according to their position along the traced path to be interpolated, but just sets `xp=linspace(0,1,len(x))`.

I.e. currently there is a missmatch between the xyz coordinates (fp) and the branchlens/pathlens (xp):

xyz1-----xyz2------xyz3-----------------xyz4--xyz5
loc1--------loc2--------loc3--------loc4--------loc5

rather than taking into account the correct pathlens

xyz1-----xyz2------xyz3-----------------xyz4--xyz5
loc1-----loc2------loc3-----------------loc4--loc5

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `9bdeeffbaf14`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.