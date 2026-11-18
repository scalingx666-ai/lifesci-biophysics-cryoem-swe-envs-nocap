# Issue

**Title:** Continuing better Viewing

## Description

The following is a copy of the remaining todos from #447

> Merging this will make    obsolete and close   probably  and I guess also 
> Will also make  and  trivial
> Also might wanna revisit  after this

> ### Comments:
> - Moved `_append_multiple_synapses` and associated functionality to `Network`.
> - `SynapseView` replaced by `module.TestSynapse.edge([0,1])`
> 
> ### Open Questions:
> - How should we name `self.view`, `self.base`, `self.at`?
> - How should we handle local indices? Should they be recomputed dynamically? I.e.`self.comp([2,3,4]).comp(1)` or to allow groups to have a local index `self.group_name.comp(0)`?
> - change name of `_cells_in_view` to `_cell_indices` etc.
> - Treat synapse trainables, i.e. edges the same as nodes? which would also allow param sharing
> - Discuss what we want to allow with new View that we didnt before -> diff of NotImplementedError and other things that are now trivial
> - How to treat local inds? Should we just have single local inds col? This would mean doing net.cell([1,2]).comp(0) would only show the first comp of cell 1 rather than the first comp in every branch of cell 1 and 2
> - should controlled_by_param be 0 or np.arange(len(self.nodes)) by default?

### Immediate Todos:
- [x] Fix broken tests
- [x] Add new tests
- [x] Add documentation and type hints
~~- [x] update existing tutorials with new API (edge)~~ Will be removed -> #463 

### Smaller Todos
- [x] locals in edges to front and globals to the back
- [x] remove local indices from edges?
- [x] interplay of nodes and edges -> delegated to #463
- [x] groups are shared per default! -> controlled by param
- [x] add test for #338

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `de1e9f22994a`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.