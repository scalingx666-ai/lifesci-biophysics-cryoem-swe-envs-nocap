# Issue

**Title:** Bugfix for networks that combine point neurons and morph neurons

## Description

Instantiating networks of mixed single comp and multi comp failed:
```python
simple_cell = jx.Cell()

comp = jx.Compartment()
branch = jx.Branch(comp, 4)
morph_cell = jx.Cell(branch, [-1, 0, 0, 1, 1])

net = jx.Network([simple_cell, morph_cell])
```

This PR fixes this.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `75ddb2b0d4c3`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.