# Issue

**Title:** New tutorials on indexing and parameter sharing

## Description

This PR adds two advanced tutorials (setting synaptic parameter values and synaptic parameter sharing). As part of this, it modifies the outline of the website and splits tutorials into `Tutorials` and `Advanced tutorials`.

To make synaptic parameter sharing possible, this PR also contributes the `.copy_node_property_to_edges()` and adds tests for it:
```python
net.copy_node_property_to_edges("global_cell_index")

df = net.edges
df = df.query("pre_global_cell_index in [3, 4]")
net.select(edges=df.index).set("Ionotropic_gS", 0.2)
```

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `0dad63ed2980`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.