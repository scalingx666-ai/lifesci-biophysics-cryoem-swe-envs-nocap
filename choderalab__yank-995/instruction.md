# Issue

**Title:** Analyze pulls from Online analysis if present

## Description

This is the realization of #979. It is not everything on that list due to some practical considerations, but here is the full list of things changed:

* Online analysis interval can now be set to checkpoint interval 
* Online analysis runs by default
* Online analysis min iterations now 200
* Checkpoint interval now 200
* Analysis now tries to get the online data by default 
* Online analysis does not attempt unbias harmonic restraints, this can be relaxed when we switch to CustomCVForces for restraint distances

Things not implemented from the issue list:
* Do not allow users to specify where the reference states are in the YAML
* Only uses the f_k as the initial MBAR guess to speed up the process, it does not use anything else. MBAR still has to be solved, but should be much faster with the online guess than not. Generalizing this so each component specifically reads entries in the online data would require re-engineering the entire analysis and online code + making users specify the reference states.

Fix #979

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `6eb6e914ac31`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.