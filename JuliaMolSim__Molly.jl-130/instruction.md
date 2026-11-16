# Issue

**Title:** Add caveat to NoseHoover docs

## Description

The code I wrote for this integrator is limited to ergodic systems as pointed out this paper:

https://www2.stat.duke.edu/~scs/Projects/REMD/NoseHooverChains1992.pdf

I will update this code at some point to use a new integration scheme as outlined in the paper below. This might be a bit beyond my capabilities in terms of math, but this is the default scheme in LAMMPS for NVT and NPT:

https://iopscience.iop.org/article/10.1088/0305-4470/39/19/S18/pdf

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `59807dd72d1d`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.