# Issue

**Title:** Convert Restraints to CV-Based

## Description

This PR converts ALL of the restraints to `CustomCVForce` objects.

This feature now requires OpenMMTools 0.16.0, which is  still in development, but the local tests pass. The OpenMMTools PR/branch which I am using is here:

choderalab/openmmtools#363

This changes many things so could use a review. Many of the changes here are reflections of the linked OpenMMTools issues

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `b97c514abaa9`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.