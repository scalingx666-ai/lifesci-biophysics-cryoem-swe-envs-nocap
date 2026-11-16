# Issue

**Title:** Grand canonical Monte Carlo tutorial

## Description

In original tutorial, the ion ration R=c_{M-}^{sys}/c_{X+,X-}^{res} spans the range 0.5 < R < 50.
At R = 50, the ion number in the system is extremely low, and the insertion acceptance is vanishingly small.
As expected, the results then depend strongly on the initial configuration and the random seed.

Description of changes:
-Added Ideal-ion (no interaction) calculation
-Implemented log-based acceptance for numerical stability, e.g. a vanishingly small acceptance rate.
-R range limited to the physically relevant region, 0.17 < R < 1.7, where the Donnan theory and the distinction between ideal and interacting ions become evident, as shown in Fig. 2a,b of the paper: "Grand-Reaction Method for Simulations of Ionisation Equilibria Coupled to Ion Partitioning" (https://pubs.acs.org/doi/10.1021/acs.macromol.0c00260).

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `12b44916da45`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.