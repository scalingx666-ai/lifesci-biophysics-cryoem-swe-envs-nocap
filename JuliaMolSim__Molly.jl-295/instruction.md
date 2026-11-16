# Issue

**Title:** Fix GB forces with salt screening and a cutoff

## Description

The GB pair energy with salt screening with the cutoff shift that Molly uses following OpenMM ([`_createEnergyTerms`](https://github.com/openmm/openmm/blob/master/wrappers/python/openmm/app/internal/customgbforces.py)), depends on  $1/f - 1/r_c$.


The forces in `gb_pair_gpol` used $1/f$ instead of $\left(1/f - 1/r_c\right)$ in the derivative of the salt term, so they didn't match the energy when both $\kappa > 0$ and a cutoff were used.
Without salt the shift is a constant and the forces were correct. This affects OBC and GBn2.

The fix passes the inverse cutoff to `gb_pair_gpol`. The self term isn't shifted, as in OpenMM.

Largest force difference (kJ/mol/nm), barnase-barstar, $\kappa$ = 0.5 nm⁻¹, 1.5 nm cutoff:

| | Before | After |
|---|---|---|
| GBn2 vs OpenMM | 1.01 | 0.00005 |
| OBC2 vs OpenMM | 1.00 | 0.00004 |

A new test compares OBC2 and GBn2 forces with `central_fdm` of the energy, with salt and a cutoff.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `da854f1fc93d`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.