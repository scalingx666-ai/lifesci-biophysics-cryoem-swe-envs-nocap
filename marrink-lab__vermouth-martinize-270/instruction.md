# Issue

**Title:** Detect N and C ter for automatic annotation of modification by comparing resid with their neighbour

## Description

This is a *partial* fix for #262. The remaining edge case is a two chain protein, in which chain terminal residues are connected through e.g. a disulfide bridge. Or a single chain protein which cyclizes through a non-peptide bond between the terminal residues.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `6aedeae7acf7`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.