# Issue

**Title:** Attribute handling

## Description

mainly to fix issues with the Go pipeline, add an attribute stash processor that stores things that shouldn't change.

Currently the Go pipeline fails for reading in contact maps for multichain proteins, because of the `MergeMolecules` that gets done to the system prior to `martinize()`, which means the resids of the input nodes are lost (#694). This PR changes the resids used to identify go contacts proper to the stashed ones, regardless of whether the contacts were found from the internal processor or an external file.

While this is just being used to sort out the Go processor, stashed attributes could also be used in other parts of the martinize pipeline

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `6183309adb81`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.