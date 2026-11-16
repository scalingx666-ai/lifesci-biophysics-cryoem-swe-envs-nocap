# Issue

**Title:** Merge cell lists in parallel.

## Description

With this PR the cell lists are merged in parallel when constructed in parallel. The performance improvements, if any, are small. But the scaling may be improved when using larger numbers of batches for cell list construction, in systems with many CPUs.

The code was somewhat simplified by making `CellList` a mutable struct, which was required to avoid the allocations caused by the closure capture.

We also split completely the inner loops of orthorrombic and tricilinic cells, which will probably make the code maintenance easier. This was also required because with the lists being merged in any order, the triclinic merging was broken because of the pair skipping rules.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `42ada90a3600`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.