# Issue

**Title:** SimulatedTempering uses ExpandedEnsembleSampler

## Description

The SimulatedTempering class is now unnecessary.  ExpandedEnsembleSampler can do all the same things and more.  I'm still keeping it around for backward compatibility, but I turned it into a thin wrapper around ExpandedEnsembleSampler.

I also updated the user guide to discuss the new classes instead.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `ce9fcace1c7c`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.