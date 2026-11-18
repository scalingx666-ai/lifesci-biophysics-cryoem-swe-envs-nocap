# Issue

**Title:** Allow changing nseg after initialization

## Description

This PR is a  and #426. It allows to set `nseg` **after** initialization of the module. This is achieved with a new `set_ncomp` method. This PR is the final requirement for a new release with these features.

### API

Works for branches:
```python
comp = jx.Compartment()
branch = jx.Branch(comp, nseg=2)
branch.set_ncomp(4)
```

...for branches within a cell:
```python
comp = jx.Compartment()
branch = jx.Branch(comp, nseg=2)
cell = jx.Cell(branch, parents=[-1, 0, 0])
cell.branch(1).set_ncomp(4)
```

...and for branches read from SWC files, in which case it reuses the xyzr to compute optional radiuses:
```python
cell = jx.read_swc(fname, nseg=2)
cell.branch(1).set_ncomp(4)
```

### Assumption and `Raises`
The `set_ncomp` function will raise an error if:  
- There are stimuli in **any** compartment in the module  
- There are recordings in **any** compartment in the module  
- The channels of the compartments are not the same within the branch that is modified  
- The lengths of the compartments are not the same within the branch that is modified 

In addition, unlike the data was read from an SWC file, `.set_ncomp()` raises an error if:  
- The radiuses of the compartments are not the same within the branch that is modified  

Finally, we currently raise `NotImplementedError` if the `Module` is a `Network`, e.g. for
```python
net.cell(0).branch(1).set_ncomp(4)
```
This is because I do not want to deal with synapse indexing for now.

We also raise a `NotImplementedError` if the `Module` is an entire `Cell`, e.g. for
```python
cell.set_ncomp(4)
```

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `4fc75a12f78e`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.