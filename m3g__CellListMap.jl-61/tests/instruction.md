# Issue

**Title:** fix: signature for pairwise mapping

## Description

Fixed signature for `map_pairwise_serial!` and `map_pairwise_parallel!` for `CellListPair` type.

Currently, the package fails for the following example:
```julia
using CellListMap

box = Box([100,100,100],20)
b1 = [[100,100,100] .* rand(3) for _ in 1:10]
b2 = [[100,100,100] .* rand(3) for _ in 1:10]
cl = CellList(b1, b2, box)

CellListMap.map_pairwise_serial!((x,y,i,j,d2,output)->output, nothing, box, cl)
```
because keyword `show_progress` is by default `show_progress` which is not defined elsewhere.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `661771ac416d`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.