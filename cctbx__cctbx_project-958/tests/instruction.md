# Issue

**Title:** Ev11 mll

## Description

New error recalibration model specific to serial crystallography merging.

The MM24 error model is a new approach to modeling intensity uncertainty in cctbx.xfel.merge and can be used in place of the Ev11 model or errors from sample residuals. This model makes several advancements from Brewster's (2019) Ev11 model:
1: The normalized deviations, the metric used to assess intensity statistics in the Ev11 error model, are replaced with pairwise differences of symmetry related and multiply measured reflections. 
2: A new error calibration formula is introduced that applies different levels of uncertainty to each observed lattice based on its correlation with the scaling reference. 
3: Optimization robustness is improved by a maximum likelihood target function.
4: Addition of diagnostic plots.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `c897b8b4fa2d`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.