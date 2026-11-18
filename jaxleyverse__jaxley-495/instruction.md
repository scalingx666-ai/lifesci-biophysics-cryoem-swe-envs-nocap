# Issue

**Title:** [MRG] Make methods/attributes private

## Description

@michaeldeistler @jnsbck is the list of methods/attributes to make private actually agreed upon? Here's what was stated int he issue:

```
"par_inds",
"child_inds",
"child_belongs_to_branchpoint",
"solve_indexer",
"cells_list",
"cumsum_nbranchpoints_per_cell",
"cumsum_nbranches"
```
I've started with `par_inds` to get a feel for the code base, but I'll wait for you input before continuing

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `0dad63ed2980`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.