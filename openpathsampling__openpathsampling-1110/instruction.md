# Issue

**Title:** Improved implementation of shooting point selector pick (avoid model that might introduce index error in subclasses)

## Description

After 5 years of using ops it bit me today, for the old implementation it was possible to have `idx == len(prob_list)` in the last iteration which would then raise an `IndexError` when trying to get the associated probability from prob_list. 

Or in other words in 5 years of ops I never tried to select the last frame of a trajectory as new shooting point, which is a good thing I would say :)

EDIT: typo

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `91318915aa0c`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.