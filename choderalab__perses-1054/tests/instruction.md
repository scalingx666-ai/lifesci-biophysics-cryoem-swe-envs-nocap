# Issue

**Title:** Fix protein mutation repex consistency tests

## Description

## Description

<!-- Describe your changes in detail. -->
This PR introduces the following changes:

For speed up:
- Decrease the number of steps per iteration from 250 to 50 
- Set `reassign_velocities` to be False 

For improved convergence:
- Set `n_iterations` = 3000 for the neutral mutations and = 3000 for the charge changing mutations
- Set `n_replicas=36` for the charge changing test
- Set `minimisation_steps` = 0, which means that the minimization will run until the energy is below a tolerance (instead of running for 100 steps, which is the default in `HybridRepexSampler`)
- Change the convergence criteria to check if DDG < 6 * dDDG

Other:
~- Change the solvent padding to be 1.7 nm (which is necessary for the nightly dev builds of openmm). **Note this may make the tests slower if we are testing against OpenMM <= 7.7**~ EDIT: We are currently testing against openmm 7.7. We should not bump up the solvent padding until we switch the GPU tests to test against openmm => 7.8 
- Clean up unfinished comment and reporter file name

## Motivation and context

<!--- Why is this change required? What problem does it solve? -->
This PR changes some of the parameters in the protein mutation repex consistency tests to ensure convergence (DDG < 6 * dDDG) and speed up the tests.

<!-- Replace ??? with the issue number that this pull request resolves. -->
 

## How has this been tested?

<!--- Please describe in detail how you tested your changes. -->
Tested on lilac.

## Change log

<!-- Propose a change log entry. -->
<!-- Examples here https://github.com/choderalab/perses/blob/master/docs/changelog.rst -->
```
Fix protein mutation repex internal consistency tests to ensure convergence and speed up the tests. So far only testing neutral transformations.
```

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `6af4bfdfc0f4`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.