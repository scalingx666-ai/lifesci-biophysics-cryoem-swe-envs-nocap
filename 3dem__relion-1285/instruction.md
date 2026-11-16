# Issue

**Title:** fix: improve numerical precision of subtomogram extraction point projection

## Description

Addresses #1284. Follows the latter (second) solution recommended in the issue description. `d4Matrix` types now do rotations with double precision. 

Thanks!

Best,
Daniel

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `cdf8c6b720d0`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.