# Issue

**Title:** Implementation of Multi-Restraint

## Description

This PR completes the Multi-restraint implementation, including the ability to control the restraints through different variables.. There are some outstanding things still.

* What is the correct way to handle the Standard State Correction for multiple restraints? It won't be additive and depends on the restrained atoms and restraints themselves.
* Making the analysis actually use the CustomCVForce features is missing. But that can be implemented separate from this PR
* I'm not sure what the best way to test this feature in a way that does not require long simulation (and I don't know what the validation would be, see the first point)

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `bdfc09d2095e`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.