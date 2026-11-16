# Issue

**Title:** Add Type Hierarchy to Interactions

## Description

Added `NBodyInteraction{N}`, `GeneralInteraction` and `SpecificInteraction` among other things. This PR is not quite enough to let me implement thermodynamic integration, but I'll try to keep the changes bite-sized.

I think we could do a lot in the future by dispatching on these, but to keep changes minimal for now and avoid breaking changes I have opted to only add a new constructor which allows passing all the interactions together, parsing them into their respective types and then calling the original constructor. 

I added the type to each interaction. I've never used the specific interactions before, but I from what I understand I do not need to mark things like `CosineAngle` or `UreyBradley` with `SpecificInteraction` as they are intended to be passed to a `SpecificAtomsInteractionList` type?

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `f54d9bc20485`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.