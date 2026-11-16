# Issue

**Title:** GPU Constraints

## Description

This PR (WIP) implements SHAKE/RATTLE constraints on GPU using the M-SHAKE algorithm. It also has angle constraints!

The traditional SHAKE implementation is not possible to parallelize easily. M-SHAKE is parallelizable but comes at the cost of the types of constraints you can have. The "fast" implementation of M-SHAKE uses an analytical matrix inverse and is therefore limited to clusters of 3 constraints or less. I do not really see this as a problem. Typically you only constrain H-bonds or the bonds of the solvent molecules. Furthermore, bonds along the back bond are not constrainable due to the assumption of a "central" atom in my current implementation. This can be removed in the future, but I think implementing rigid body dynamics is the correct way to constraint a backbone and not SHAKE.

The types of clusters supported:
- Single bond
- 1 central atom bonded to 2 other atoms
- 1 central atom bonded to 3 other atoms
- 1 central atom bonded to 2 other atoms with the angle constrained

This leaves out things like CH4.

The current implementation is complete for all RATTLE variants and for SHAKE with a single bond as these are all analytical solutions and do not require iteration. The 3 remaining SHAKE kernels require iteration and I am not sure how best to handle that as load balancing is important for GPU performance. Open to suggestions as to how that should be done.


Some extra work will be required to actually support angle constraints. Maybe we leave this to another PR, and I just throw an error if I detect angle constraints.
- [x] Update things like n_dof which assume there are no angle constraints
- [x] Update build_clusters for angle constraints

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `9feb5faa9936`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.