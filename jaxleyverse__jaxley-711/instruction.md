# Issue

**Title:** Bugfix for indexing in `init_states` for networks

## Description

To reproduce the error:

```python
from jaxley.channels import HH

comp = jx.Compartment()
branch = jx.Branch(comp, ncomp=2)
cell1 = jx.Cell(branch, [-1, 0, 1])
cell2 = jx.Cell(branch, [-1, 0, 1, 2])
cell3 = jx.Cell(branch, [-1, 0])
net = jx.Network([cell1, cell2, cell3])

net.cell(0).branch(1).add_to_group("apical")
net.cell(1).branch(3).add_to_group("apical")
net.cell(2).branch(1).add_to_group("apical")

net.apical.insert(HH())

net.init_states()
```

The reason that this bug happened is that `init_states` was still using the `global_comp_index`, instead of the `index` (as is done in `step_channels`).

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `d84b3221c1dc`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.