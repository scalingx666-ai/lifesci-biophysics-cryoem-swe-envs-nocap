# Issue

**Title:** Cut the 0.19.0 release

## Description

* Requires minimum version of OpenMMTools 0.13.3 for critical fix in SamplerState
* Changed from Schema to Cerberus for YAML data validation
* Adds more general region selection schema for Topography and atom selection
    * Establishes the future framework for SMARTS selection

@andrrizzi could you double check this has all the features we want?

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `29ee6d8a11ab`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.