# Issue

**Title:** Rama residues

## Description

Updated the ramachandran tool so it writes the resid for each entry in its output, in addition to the frameno. Also added comment about the secondary structure binning's origin citation to the fullhelp message.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `af3e543fe5ed`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.