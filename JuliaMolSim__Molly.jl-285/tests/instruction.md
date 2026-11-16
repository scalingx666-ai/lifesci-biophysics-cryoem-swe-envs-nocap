# Issue

**Title:** Add MetaDynamics

## Description

Add enhanced sampling via MetaDynamics simulations implemented as a general force field.

MetaDynamics computes collective variables at fixed intervals and adds a Gaussian potential to the memory of the visited cvs with a frequency according to _deposit_interval_. The memory is implemented as subtype of _AbstractMetaDynamicsMemory_:
- _ListHills_: Lists of cv-positions, height and variance. Exact method that needs summing over all previous hills in vicinity.
-  _GridHills_: Discretized cv-space spanning grid_min:bin_width:grid_max. Not exact but O(1) evaluation cost
- Can be extended by none equidistant grids or adaptively refined grids for less space requirements for higher dimensions

The bias force is evaluated at each step from the time dependent memory bias according for each cv. Tempering is implemented as a subtype of _AbstractTempering_. 
- _NoTempering_: Heights of gaussians is constant with time. This can easily lead to biases that are far larger than the free energy potential differences.
- _WellTemperedTempering_:  Heights of the gaussians decay with fixed rate to limit the maximal bias.
- interface for _tempering_height_ allows to implement cv-, bias-, memory- and height-dependent schemes if wanted.

I intend to add more CVs that are commonly used in [plumed](https://www.plumed.org/doc-v2.10/user-doc/html/_colvar.html) to explore the conformational ensemble of a protein.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `f4d31697b3aa`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.