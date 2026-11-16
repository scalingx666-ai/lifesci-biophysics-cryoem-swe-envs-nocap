# Issue

**Title:** RSR weight: use the single chunk estimate instead of falling back to the default of 50

## Description

In `mmtbx/refinement/real_space/weight.py` the overall weight is the mean of the lower half of the sorted per-chunk estimates. With exactly one estimate (small models: fewer than 20 protein/NA residues, or no protein/NA chain at all) that half is empty, so `mean_default` returned the default of 50 and the actual estimate was discarded. This over-weights the map and distorts geometry on small ligand-sized models (user report: bond RMSD 0.15 A, O-P-O angles ~49 deg with automatic weight vs 0.006 A / 114 deg with weight=1).

Fix: keep at least one estimate; the default is used only when there are no estimates at all. Behaviour with two or more chunks is unchanged.

Also fixed the "random chunks:" summary line, whose format string had no placeholder so the list never printed.

Added a single-chunk case to `tst_weight.py`. Verified with tst_weight, tst_individual_sites_1/2/3, tst_rigid_body, the NCS real-space minimization test, and all phenix_regression real_space_refine tests.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `b897de9a4d9f`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.