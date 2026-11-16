# Issue

**Title:** Minimize InitialStates only considering fully interacting atoms for the min_cutoff threshold

## Description

* Adds an optional field to `InitialState` to store the `AtomMapMixin` (or really the SingleTopology). Still allows unpickling older files since the field is optional.
* In the case of intermediate windows the dummy atoms can move all over the place and could see failures where min_cutoff assertion was triggered for non-interacting dummy atoms. 
* Sets min_cutoff == 0.0 for vacuum cases we had missed in the tests, to be consistent with `run_vacuum`.
* The definition of interacting atoms is entirely determined by the atom mapping

  * Had attempted to do this by looking at the parameters, but seems more error prone and was not immediately obvious how to handle NonbondedPairListPrecomputed parameters.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `5e9d8c1c66a6`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.