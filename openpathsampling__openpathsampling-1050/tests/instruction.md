# Issue

**Title:** add simstore table access via slice

## Description

Today, I got annoyed today that I could not do
`steps = storage.steps[:100]` in some test code using simstore.

This adds handling of slices in the `__getitem__` logic. 
It behaves as a `list` would do (so an empty list on an empty slice, a list with a single item for slices like `[4:5]`, a single item with `int` and a `TypeError` if it is not an `int` or `slice`)

It should also leverage batched loading, if I understood the code correctly

One assumption that I added is that the `name` of objects can only be strings (for `_name_to_uuid`), I don't know if this is currently an assumption/requirement, but all tests seem to pass on this.

This code is `python > .3.6` (f strings) and the tests are `python > 3.7` (leverages dict order)

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `d2488988059f`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.