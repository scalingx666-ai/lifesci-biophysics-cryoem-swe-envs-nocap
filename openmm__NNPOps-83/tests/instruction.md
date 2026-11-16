# Issue

**Title:** Fix a bug the periodic boundary conditions in ANI SymmetryFunctions

## Description

This aims to fix #82 
It sets the `periodic` flag  in SymmetryFunctions.cpp based on if the cell vectors are defined as suggested by @peastman 

I have added a new test case that is a water box with PBCs, this is run in the TestOptimizedTorchANI.py and TestSymmetryFunctions.py tests where the energy and forces of the NNPOps implementation are compared to the TorchANI implementation.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `3c96f5be36e9`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.