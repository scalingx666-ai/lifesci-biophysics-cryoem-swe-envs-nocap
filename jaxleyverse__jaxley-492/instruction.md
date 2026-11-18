# Issue

**Title:** Fix synapse clamping

## Description

Clamping of synaptic states was not tested and slipped through the cracks when #447 was merged.  This lead to several issues #485.

This PR fixes this and adds test coverage.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `ca6dbbc37949`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.