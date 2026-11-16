# Issue

**Title:** diffBragg: fix out-of-range index when syncing panel origin refine flags

## Description

<!--
Intermittent failure on libtbx.run_tests_parallel
libtbx.python modules/cctbx_project/simtbx/ diffBragg/tests/tst_segfault.py --panel y
-->

<!-- Segmentation fault debugged by Claude and fix proposed -->

In add_diffBragg_spots, the panel-origin loop compared each manager's refine_me against db_flags.refine_panel_origin[i_pan_orig], where i_pan_orig takes values 1..3, instead of [i_pan] (0..2). For the origin Y component (refine id 16) this read index 3 of a 3-element std::vector<bool>, i.e. an uninitialized padding bit. When that bit was set, the flag was never updated, first_deriv_imgs.panel_orig was never allocated, and the post-kernel increment_image wrote into an empty vector, causing an intermittent segfault. For X and Z the wrong slot happened to be false, so they worked by accident; refining several origin components together could still set the wrong flags.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `cb6f6e2513bb`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.