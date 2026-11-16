# Issue

**Title:** Refactored MD17 DataModule and added MD22 datset

## Description

Added a base `GDMLDataModule` from which datasets like MD17 can be derived. 
Refactored the MD17 data module to account for this change and added a custom data module for the new MD22 dataset (http://www.sgdml.org/).

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `94e7dc809b1a`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.