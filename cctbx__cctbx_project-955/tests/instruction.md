# Issue

**Title:** adds option to store average wavelength per pixel

## Description

this shouldnt affect any of the exafel workflows, and changes no defaults.. Simply adds the option to store the average hkl visited by the kernel in each pixel (important for Laue work currently with Hekstra lab)

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `b959a753e132`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.