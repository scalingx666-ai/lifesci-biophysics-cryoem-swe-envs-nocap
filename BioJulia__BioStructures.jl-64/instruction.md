# Issue

**Title:** Use correct indices of residues in Transformation

## Description

Sorry I didn't catch this earlier together with #63. The documentation of `Transformation` states that its fields `inds1` and `inds2` store the indices of residuals that were used when superimposing structures. However, when residues were skipped due to differing amounts of atoms, the corresponding indices were still part of `inds1` and `inds2`. This PR makes sure that this is no longer the case.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `2bf28ef41727`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.