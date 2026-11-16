# Issue

**Title:** Created Coulomb prior

## Description

This adds a Coulomb interaction.  At the moment it requires partial charges to be specified by the dataset.  In the future we'll want to let the model predict charges, but that will come later.

The interaction is applied to all pairs without any cutoff.  The point of this prior is to provide accurate long range interactions.  It does get scaled by `erf(alpha*r)`, which reduces the effect at short ranges and prevents it from diverging at r=0.  That also happens to be the scale factor of the reciprocal space term in Ewald summation.  Eventually we'll want to apply this to large systems that are partly modeled with ML and partly with a conventional force field.  The idea is that you'll be able to include the Coulomb prior simply by including the ML region in the reciprocal space PME calculation.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `9ab6280714aa`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.