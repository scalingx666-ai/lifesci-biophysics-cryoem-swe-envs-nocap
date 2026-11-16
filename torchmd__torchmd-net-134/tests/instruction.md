# Issue

**Title:** Implement ZBL potential

## Description

This is the first piece of #26.  It isn't fully tested yet, but I think it's ready for a first round of comments.

I created a mechanism for passing arguments to the prior.  `prior_args` is now the option specified by the user in the config file.  `prior_init_args` stores the value returned by `get_init_args()`, which contains the arguments needed for reconstructing it from a checkpoint.

This prior requires the dataset to provide several pieces of information.  To keep things general, the HDF5 format allows the file to contain a `_metadata` group which can store arbitrary pieces of information.  Most of the other dataset classes should be able to hardcode the necessary values, since they aren't intended to be general.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `da617ea0b79f`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.