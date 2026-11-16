# Issue

**Title:** probe2: make dot output repeat between runs

## Description

Two things made probe2's raw and JSON dot order, and which alternate a dot is
labelled with, vary between runs:

- `DotInfo` caches its sort-key names by `memory_id()` and never cleared the
  cache, so a later model in the same process could reuse an address and sort
  on another atom's name. The cache is now cleared with each model's results;
  it had also grown with every model.
- `check_dots()` keeps the first of equally close atoms (one atom's alternates
  at a single position), and the candidates came from a Python set. They are
  now passed in `i_seq` order.

With both, the same model gives byte-identical output across processes.
`mmtbx/regression/tst_probe2_name_cache.py` fails without the cache fix.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `f2bfb926b03a`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.