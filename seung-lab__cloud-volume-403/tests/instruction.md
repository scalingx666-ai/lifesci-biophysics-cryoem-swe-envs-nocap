# Issue

**Title:** fixing level=2 for bounding box queries

## Description

When doing a bounding box query with graphene, it doesn't make sense to specify something other than level2 because you don't want to get stitched fragments that extend well beyond your bounding box in some inconsistent fashion depending on the state of mesh stitching in the backend. 

This simply sets the level=2 when specifying a bounding box. 

I have a corresponding PR on the PCG backend to make this functional.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `28928c9ed508`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.