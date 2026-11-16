# Issue

**Title:** Handle Water Sampling with AbsoluteFreeEnergy

## Description

* Low priority bug where we can't run water sampling with InitialState objects constructed from AbsoluteFreeEnergy. User can work around this themselves by unwrapping the summed potential, but thought I would put up a quick fix. 
* Unable to find NonbondedInteractionGroup triggers failure due to wrapping up with a SummedPotential

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `97106b691cb1`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.