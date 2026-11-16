# Issue

**Title:** Add DisplacementLogger

## Description

This PR adds an array of image flags which are updated by the `wrap_coordinates` function. This enables calculating displacements even when atoms move over the box boundary. This allows us to calculate things like MSD and displacement. 

I also added two loggers `ImageFlagLogger` and `DisplacementLogger`. I might also update the RMSD calculation in analysis.jl to reflect this change.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `0a04a4c78142`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.