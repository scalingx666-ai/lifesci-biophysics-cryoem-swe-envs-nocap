# Issue

**Title:** Parallelize PIMD NVE and Langevin integration

## Description

### Summary

Parallelize the GPU implementation of PIMD NVE propagation and Langevin integration to improve scaling with bead count while preserving existing simulation behavior.

### Changes

* Parallelize PIMD NVE normal-mode propagation.
* Parallelize Langevin normal-mode transformation and thermostat updates.
* Preserve the existing CURAND state and random-number consumption order.
* Add reusable normal-mode position and velocity workspaces.

### Validation

* Tested PIMD, RPMD, TRPMD, Eco-PIMD, NPT-PIMD, and PIMD-SCR with up to 128 beads.
* All 182 unaffected regression cases and all 23 regression relations passed.
* PIMD thermodynamic outputs were bitwise identical; remaining `final.xyz` / `restart.xyz` differences were limited to floating-point roundoff, with a maximum absolute difference of `6.94e-18`.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `c76ae0642920`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.