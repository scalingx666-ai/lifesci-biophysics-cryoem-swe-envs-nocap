# Issue

**Title:** Add Ramachandran errorbar plot and per-residue frame plots

## Description

- Combine phi/psi means into an average Ramachandran plot with horizontal and vertical std error bars
- When residues are selected, also generate per-residue frame-level Ramachandran scatter plots
- Keep per-residue plot paths in results for downstream use

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `b7103a90ce4d`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.