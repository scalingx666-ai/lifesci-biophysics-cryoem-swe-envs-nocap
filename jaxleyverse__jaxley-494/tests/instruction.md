# Issue

**Title:** Add support for boolean indexing of cells/branches/comps

## Description

This PR adds boolean indexing to Views. This is for convenience. Can be used to do stuff like:
```python
r_greater_1 = net.nodes.groupby("global_cell_index)["r"].mean() > 1
net[r_greater_1].nodes.vis()
# or
net.select(nodes=net.nodes["r"] > 1) # vs. net.select(nodes=net.nodes.index[net.nodes["r"] > 1])
```

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `fd3e2b27faea`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.