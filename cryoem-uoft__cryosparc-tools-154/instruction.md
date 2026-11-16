# Issue

**Title:** feat!: querying a non-existant field raises KeyError

## Description

Breaking change.

Right now, querying a dataset with a non-existent field returns the entire dataset. I find this counterintuitive and have been bitten by it several times. For example, trying to filter particles with a typo of `location/mcrograph_uid` instead of `location/micrograph_uid` results in *all* particles in the dataset returning. Python (e.g., `dict["key"]` vs. `dict.get("key")`), popular dataframe packages (e.g., pandas), and numpy (structured arrays) all raise exceptions of some flavor when attempting to access data with a key that's not in the object.

This commit changes the behavior of `Dataset.query_mask()` to raise a KeyError when attempting to query non-existent fields, and lists the missing fields in the exception message. I also removed the test for the old behavior and added a test for the new behavior.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `27954f06e7e8`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.