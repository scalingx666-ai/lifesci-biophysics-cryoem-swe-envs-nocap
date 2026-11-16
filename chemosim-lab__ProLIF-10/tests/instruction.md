# Issue

**Title:** Issue #8: Better support for DNA residues

## Description

Fixes issue #8 

- Fixes the `ResidueId.from_string` regex to support residues codes of 1 to 3 letters (RNA, DNA and proteins)

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `8e4d2f256c13`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.