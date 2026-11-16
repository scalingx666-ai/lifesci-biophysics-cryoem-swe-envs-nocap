# Issue

**Title:** Mini changes

## Description

Pulling relevant commits from  because that PR is _old_ and contains a lot of unnecessary commits.

- For trajectory.atom_slice, if a residue's resSeq == 0, the line 93 reassigns resSeq as residue.index. Fixes that bug.
- Edited/Added two residue names 'HSD' and 'HSE' to the residue name list.

I didn't do anything except cherry-picking the commits and fixing the merge conflicts.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `22544580ef71`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.