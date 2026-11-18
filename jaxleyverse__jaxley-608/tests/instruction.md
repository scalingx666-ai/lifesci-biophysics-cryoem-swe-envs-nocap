# Issue

**Title:** Allow intersections of groups

## Description

This PR makes it possible to do `module.group1.group2`. For example:

```python
net.cell("all").branch(0).add_to_group("soma")
net.cell([1, 2, 3]).add_to_group("exc")

net.exc.soma.nodes  # Shows somata of excitatory neurons.
```

. Thanks @fololo for raising this!

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `143b9a2a678b`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.