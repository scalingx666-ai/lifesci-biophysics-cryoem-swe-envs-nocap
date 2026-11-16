# Issue

**Title:** Ensure that SimStore always saves movescheme

## Description

Based on an email report from @gyorgy-hantal. It seems that SimStore was not saving the move scheme in the rare case that all steps fail due to maximum length/NaN. This seems to be because of a difference in SimStore allowing you to stash several steps for saving, whereas netcdfplus immediately saves after each step.

This PR also introduces a (not well-tested) `unpatch` method to undo the SimStore monkey patches. This will mainly be useful in testing.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `af6be698fdb2`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.