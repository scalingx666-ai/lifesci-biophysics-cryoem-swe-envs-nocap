# Issue

**Title:** Improve Ramachandran plots with error bars and per-residue frames

## Description

Summary
- Combine phi/psi means into an average Ramachandran plot with horizontal and vertical std error bars
- When residues are selected, also generate per-residue frame-level Ramachandran scatter plots
- Keep per-residue plot paths in results for downstream use

Tests
- pytest tests/test_dihedrals_residue_options.py tests/test_analysis_basic.py -q

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `353aad5d4a7d`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.