# Issue

**Title:** feat: Adding stratified sampling as the default diversity selection method.

## Description

Motivation: old diversity selection is taking a long time, and it is the bottleneck to NANI calculation. To make it truly scalable for ultra-large simulation, this step is to be revised. 

Feature: added stratified sampling to be the default diversity selection.

Effects: The NANI algorithm has been revised. The results are still robust and comparable to the paper and was tested on the HP35 systems for benchmark. This is the only clustering where the new diversity selection (aka strat sampling) is implemented. All other clustering remains the same diversity selection as previously. The variable may have changed to adapt the current version of diversity selection. 

Tests: new unit tests have been written to reflect this change in BTS and NANI. New test files have been added as well.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `0aa581682691`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.