# Issue

**Title:** Update automatic equilibration detection to handle SAMS

## Description

This PR makes changes to the multistate sampler analysis code to allow automatic equilibration detection to work with SAMS and other methods that adjust log weights dynamically.

@lnaden: I had to roll back a bit of the generalization of the `u_n` negative log deviance timeseries fed to automated equilibration detection because I couldn't find a good way to preserve this in generality. We can figure out a better way to generalize that component later. The rest of the infrastructure worked well, though!

I also had to implement a few hacks that access some online analysis data directly, since I couldn't figure out how to (1) check if the online analysis data existed, and (2) retrieve particular parts of it through the existing `Reporter` API. This could undoubtedly use refinement.

This hasn't been extensively tested beyond the unit tests. I'm brainstorming fast but useful tests, as well as testing on `lilac` right now.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `1b8bfd977d14`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.