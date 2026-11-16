# Issue

**Title:** Add some type check in create_model

## Description

When load_model or create_model are called explicitly there is no mechanism to enforce types of the arguments like it happens when train.py is used (via arg parse).

This means that something like:
```python
model = load_model(..., cutoff_upper=5)
```
fails because cutoff_upper should be a float but it is passed down as an integer.

This PR casts to float when necessary to avoid this.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `af64cdb94769`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.