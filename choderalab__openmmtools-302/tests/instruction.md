# Issue

**Title:** Update to 0.13.2 release

## Description

Cut 0.13.2

In this Release:
* Added support for `SamplerState` slicing (#298)
* Added bit operators `and` and `or` to `math_eval` (#301)

@andrrizzi anything else you are wanting to get merged into this before the cut?

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `b1e37d260fed`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.