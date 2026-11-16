# Issue

**Title:** Fix Water Sampling When REST Is Enabled

## Description

The water sampler needs to produce the same nonbonded interaction energies as used in MD to ensure correct sampling. Because of the Ligand-Env split with the `NonbondedInteractionGroup`, we have to merge the ligand parameters of the `NonbondedInteractionGroup` and the host environment of the `NonbondedAllPair` to get the correct parameters. The `NonbondedAllPair` contains all of the host parameters, but does **not** contain ligand parameters.

Prior to REST we would merge the protein parameters from the NonbondedAllPair potential into the parameters of `NonbondedInteractionGroup`. When REST was introduced we chose to modify both the ligand and environment parameters, which resulted in the `TIBDExchangeMover` softening all of the waters (but not protein) which produced an inconsistency between the water sampling and MD. This PR resolves this inconsistency and adds assertions to avoid the issue in the future.

## Changes
* Take all of the environment parameters from `NonbondedAllPair` and only use `NonbondedInteractionGroup` for the ligand parameters
* REST no longer modifies the environment parameters, only modifying the ligand parameters to provide a canonical source of parameters. There is some debate about this, and in the future we may want to adopt a different approach in the future.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `ea943e17a80c`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.