# Issue

**Title:** FIxing modeller bug for WT case

## Description

Modeller was throwing an error when given `WT` in the yaml pipeline. 

* fixing the way this is being handled 
* Adding a test for the WT case

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `da5e152872a2`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.