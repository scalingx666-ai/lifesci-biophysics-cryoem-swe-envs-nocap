# Issue

**Title:** Carry over groups from constituents of a module

## Description

Fixup for things like:
```python
cell1 = jx.Cell()
cell1.add_to_group("soma")

cell2 = jx.Cell()
cell2.add_to_group("axon")

net = jx.Network([cell1, cell2])
net.soma
```

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `e503c61d3f48`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.