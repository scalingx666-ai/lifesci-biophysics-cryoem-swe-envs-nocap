# Issue

**Title:** Bugfix in Nose-Hoover chain integrator

## Description

@andysim and I realized that constraints had not been taken into account when calculating the number of degrees of freedom in the `NoseHooverChainVelocityVerletIntegrator`. 
As a result, the system temperature did not converge to the target value for systems with constraints.

I added a test to make sure that the target temperature is now matched.

The fix changes the API of the constructor: It requires the system to be passed as an argument (only in order to extract the correct number of constraints; the integrator then forgets about the system).

Cheers,
Andreas Krämer

 - [x] Implement feature / fix bug
 - [x] Add [tests](https://github.com/choderalab/openmmtools/tree/master/openmmtools/tests)
 - [ ] Update [documentation](https://github.com/choderalab/openmmtools/tree/master/docs) as needed
 - [ ] Update [changelog](https://github.com/choderalab/openmmtools/blob/master/docs/releasehistory.rst)

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `ee7724296f16`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.