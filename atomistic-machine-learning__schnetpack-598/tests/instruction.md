# Issue

**Title:** provide batch size to self.log() of lightning module to prevent warning

## Description

this gets rid of annoying warnings of the form
```
/usr/local/lib/python3.11/site-packages/pytorch_lightning/utilities/data.py:77:
Trying to infer the `batch_size` from an ambiguous collection.
The batch size we found is 100. To avoid any miscalculations,
use `self.log(..., batch_size=batch_size)`.
```
I assume/hope `batch['_idx']` is always available?

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `caa3acf77522`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.