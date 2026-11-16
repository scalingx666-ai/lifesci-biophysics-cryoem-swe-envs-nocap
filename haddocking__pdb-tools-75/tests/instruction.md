# Issue

**Title:** [FEATURE] Added new tool to assign unique names to HETATM atoms + tests

## Description

Addresses #71 

Note the [FEATURE] tag on the PR name. Should be pushed to the main branch with this name to trigger a minor version change (so, 2.1.0 instead of 2.0.8) because of the new tool!

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `37008a8eb006`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.