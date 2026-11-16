# Issue

**Title:** Make ELC tuning follow literature error-formula

## Description

While searching for the error-formulas for ELCIC I noticed, that during the refactorings a long time ago I introduced an error in the tuning, which causes both the ELC and ELCIC to tune to slightly too small summation limits, but nothing wrong enough we caught in our CI.

I compared the energy and force results to a similar system as we have in our testsuite, the energy-distance curve of a single dipole in front of an infinite dielectric wall, which came up during the ELCIC Python reimplementation work currently in progress. (Script: [elc_analytic.py](https://github.com/user-attachments/files/26384451/elc_analytic.py)) Because I shrank the system significantly compared to our testsuite, a 2D Ewald sum is used as a comparison to include the contribution of the periodic images, which I verified for its correctness for the energies. The force-code for the Ewald is only generated and **unverified**, so I wouldn't take the reference forces too seriously without further verification, however, I included them for completeness, and to document my surprise about how the force-differences look like very close to the surface.

The results without my PR:
[elc_analytic_current.pdf](https://github.com/user-attachments/files/26384687/elc_analytic_current.pdf)

and after my PR:
[elc_analytic_new.pdf](https://github.com/user-attachments/files/26384682/elc_analytic_new.pdf)

One can see that we are slightly closer to our requested pairwise errors for the largest accuracy I selected, but mainly, we follow the equation in the paper again (noting that our `tuned_far_cut` is already scaled by $1/L$) and with the exception that the formula in the paper is written for a cubic box of length $L$, which is why our `sum`-part in the code is slightly, but reduces to the one in the paper for a cubic box.

I also verified that this does not influence #3001.

Description of changes:
- Made the ELC error formula follow the literature equation in https://pubs.aip.org/jcp/article/117/6/2496/459826/Electrostatics-in-periodic-slab-geometries-I

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `b4a5b0350213`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.