# Issue

**Title:** make neighbor list search with CellListMap.jl non-allocating

## Description

@jgreener64 

I am just opening the pull request so that the changes are easier to visualize. Needs polishing and fixing of the Int type for the tests to pass.

I have changed a little bit the implementation of the neighborlist structure to update in place the lists and cell lists from step to step in the simulations. This should bring some performance benefits (and much less memory usage).

Most tests are passing, but some are failing when you change from Int64 to Int32 integer types (I'm unsure where is this being defined, and it does not seem to be ready to be propagated to the type of neighbour list.

Otherwise the tests seem to be passing. If you want to take a look at this, and see if in the cases where it runs (Int64) the performance is improved, I can polish what is missing to make everything to work.

The current approach (in which NeighbourList is a structure that contains both the number of pairs and an array of pairs that does not need to be replaced each time) can be used for the other approaches of computing neighbor lists, with the same advantages.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `e11bd4b22e09`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.