# Issue

**Title:** Add `exclude_keys` option to `NequIPLMDBDataset `

## Description

Feel free to close if this isn't wanted.
Just a drop-in addition for an `exclude_keys` option with `NequIPLMDBDataset` (useful if dealing with massive LMDB datasets where re-processing to remove certain keys can take a while). 
Tested and worked well for me.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `4f41ab0630f0`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.