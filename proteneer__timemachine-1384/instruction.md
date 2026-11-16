# Issue

**Title:** remove extraneous summed potential now that the L-E ixn groups are merged

## Description

- Remove SummedPotential for SingleTopology class
- Look up NonbondedInteractionGroup directly
- This was missed in

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `edec1389cf2e`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.