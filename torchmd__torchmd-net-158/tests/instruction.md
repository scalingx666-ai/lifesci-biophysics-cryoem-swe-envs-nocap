# Issue

**Title:** Allow datasets to pass extra fields to models

## Description

This implements the feature described in   The dataset can store arbitrary extra fields in the Data objects it returns.  They get passed to the model in the `extra_args` parameter.  At the moment, the model only passes them on to the priors, since that's the initial thing I need them for.  It would be just as easy to pass them to the representation model and output model, if we find we need that in the future.

As an example of using this, I made the HDF5 dataset class support an optional `partial_charges` field.

We might consider getting rid of the hardcoded `q` and `s` arguments (I couldn't find any existing models that use them) and letting them get passed in `extra_args` if they're ever needed.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `7ea833d8ab1e`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.