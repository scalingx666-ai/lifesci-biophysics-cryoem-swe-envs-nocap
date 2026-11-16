# Issue

**Title:** Add Checkpointing of Trailblaze Algorithm

## Description

## Description
This PR aims to implement a basic checkpointing system for the trailblazing algorithm - currently if the algorithm is interrupted while running (e.g. if an insufficient wall clock limit is set by a job submission script), the partially computed optimized lambda states are lost.

## Status
- [x] Ready to review
- [ ] Ready to go

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `4dfcc8e127c5`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.