# Issue

**Title:** Test additional MCS pruning heuristics if `connected_core = True`

## Description

This PR adds an additional pruning heuristics that checks and see if a given atom-mapping will result in a disconnected core.
The algorithm works as follows: 

1. Given a graph `G`, and a set of nodes `N` and edges `E`, partition `N` into three groups: `A` is mapped vertices, `B` is de-mapped vertices, and `C` is unvisited vertices. A de-mapped vertex is a vertex that has been visited but is explicitly omitted from the atom-mapping. 
2. Let `P` be the union of `A` and `C`. Note that this may form disconnected components.
3. Enumerate the connected components on the subgraph of `G` induced by `P`: `cc_1, cc_2, ...`
4. If `A` is entirely a subset of one of `cc_1, cc_2, etc`, then the atom-mapping can still be connected down-stream. Otherwise, `A` has already been fragmented into multiple connected components and the atom-mapping is guaranteed to result in a disconnected core.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `d7ecc3805a19`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.