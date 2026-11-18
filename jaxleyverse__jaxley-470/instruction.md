# Issue

**Title:** Write trainables to module

## Description

This PR introduces a `write_trainables()` which adds the trainables to the `.nodes` and `.edges` such that they can be used for plotting, printing,...

```python
net.make_trainable("radius")
params = net.get_parameters()

net.write_trainables(params)
```

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `75f7fb0c696c`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.