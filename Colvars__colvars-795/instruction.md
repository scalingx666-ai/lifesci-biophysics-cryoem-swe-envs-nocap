# Issue

**Title:** Account for NAMD's GlobalMasterFrequency in ABF

## Description

To fix #793 :

- [x] properly update the timestep number when GlobalMasterFrequency is not 1
- [x] propagate extended Lagrangian dynamics correctly when GlobalMasterFrequency is not 1

Time-dependent biases may not assume they get updated every timestep anymore - check:
- [x] ABF (already MTS aware)
- [ ] metadynamics
- [ ]  moving restraints
- [ ] ALB
- [ ] others?

Also:
- [x] enforce that global output frequencies are commensurate with GlobalMasterFrequency

EDIT: this was merged before completion and thus contains only the ABF part

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `d06a03066d86`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.