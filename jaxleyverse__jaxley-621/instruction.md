# Issue

**Title:** View tracks ._comp_edges

## Description

After this PR, `._comp_edges` gets updated in `View`:
```python
cell.branch(1).loc(0.0)._comp_edges
```

This PR also introduces a few special rules for this:
- for `cell.branch(1)` all edges to neighboring branchpoints are included.
- for `cell.branch(1).comp(0)` no edges to branchpoints are included.
- for `cell.branch(1).loc(0.0)` edges to the parent branchpoint are included.
- for `cell.branch(1).loc(1.0)` edges to the child branchpoint are included.

Why do we need these special rules? For modifying a morphology, we need some method to indicate to which end of a branch we want to attach another morphology. Consider the case where all branches have 1 compartment. Then:
`morph_attach(cell.branch(0).comp(0), new_morph)` would not know whether to attach the `new_morph` at the end or at the beginning of the `comp(0)`. This is now possible by simply distinguishing between
- `morph_attach(cell.branch(0).loc(0.0), new_morph)`
- `morph_attach(cell.branch(0).loc(1.0), new_morph)`

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `8012cde20c35`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.