# Issue

**Title:** TestSystems for Double Well in WCA Fluid

## Description

This PR adds (two generalizations of) a test system that has been frequently used in rare events. I'm calling these `DoubleWellDimer_WCAFluid` and `DoubleWellChain_WCAFluid`. In both cases, we start with the existing `WCAFluid` test system, and add bonded interactions of the form $h (1 - ((r - r_0 - w) / w)^2)^2$. This is a symmetric potential with a minimum at $(r_0, 0)$, a maximum at $(r_0+w, h)$, and a second minimum at $(r_0 + 2w, 0)$. The widely-used test system adds this interaction between 2 atoms, so there is one dimer in the WCA fluid.

For the `DoubleWellDimer_WCAFluid`, the system will create bonds between pairs of atoms, so the bonds are created between atom 0 and 1, atom 2 and 3, etc., resulting in multiple separated dimers, each dimer connected by the double-well potential. This is similar to what was done in [Swenson and Bolhuis, JCP 2014](http://doi.org/10.1063/1.4890037). The standard example is `ndimers=1`.

For the `DoubleWellChain_WCAFluid`, the system will create a chain of bonds of the desired length, so bonds are created between atom 0 and 1, atom 1 and 2, etc. For `nchained=1`, this is just the WCA fluid; for `nchained=2`, this is identical to the `DoubleWellDimer_WCAFluid` with `ndimers=1`, and for `nchained=3` this is the same as the potential used in [Rogal and Bolhuis, JCP 2008](https://doi.org/10.1063/1.3029696).

The clever bit in the implementation is to recognize that the same algorithm works for both; the only difference is in which atom pairs are selected to have the custom bond force added to them.

 - [x] Implement feature / fix bug
 - [x] Add [tests](https://github.com/choderalab/openmmtools/tree/master/openmmtools/tests)
 - [x] Update [documentation](https://github.com/choderalab/openmmtools/tree/master/docs) as needed
 - [x] Update [changelog](https://github.com/choderalab/openmmtools/blob/master/docs/releasehistory.rst)

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `042c27fa00e2`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.