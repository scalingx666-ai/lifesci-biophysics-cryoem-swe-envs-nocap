# Issue

**Title:** FIRE minimizer and MPI support for Cray XK nodes

## Description

On `lilac`, minimization of 50K atom systems (like c-Met) often took ~30 minutes/minimization, with several stages of minimization required (fully-interacting system, alchemically-modified system) to start a run. 

This PR implements support for a tuned version of the FIRE minimizer as a `CustomIntegrator`, now enabled by default:

Erik Bitzek, Pekka Koskinen, Franz Gaehler, Michael Moseler, and Peter Gumbsch.
Structural Relaxation Made Simple. PRL 97:170201, 2006. [DOI](http://dx.doi.org/10.1103/PhysRevLett.97.170201)

For c-Met, this brings minimization times down from 30 minutes to 30 seconds.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `beedbe730a76`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.