# Issue

**Title:** Init params

## Description

This PR adds an `init_params` method to channels.
```python
cell = jx.Cell()
cell.insert(HH())
cell.set("celsius", 27.0)  # Reduce temperature by 10 degrees.
cell.init_params()
cell.nodes
```

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `fb119fd20679`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.