# Issue

**Title:** Optimize the GPU pairlist of the selfcoordnum kernel

## Description

It looks like that the Github stacked PRs do not support PRs across forks, so I have to create this PR targeting `master`. **This PR depends on 

This PR uses bit fields to store the pairlist data of `selfcoordnum`, and implements the pairlist in a tile-based manner. Specifically, the atoms are grouped in tiles, and the tile size matches the warp size (NVIDIA) or wavefront size (AMD) of the GPU. The following struct (see `colvarcomp_coordnums_kernel.h`) is used to record the interacting atom pairs between two tiles:
```c++
template <int tileSize>
struct TilePairMask {
  using PairMaskT = std::conditional_t<(tileSize <= 32), uint32_t, uint64_t>;
  PairMaskT pairMask[tileSize];
};
```
This design brings two benefits:
- Reduced memory footprint. For an atom group of 65,536 atoms, the CPU pairlist implementation would consume about 2 GiB of memory. The old GPU implementation has the same issue. After using the bit fields, the memory usage should drop to about 256 MiB.
- The original CPU pairlist implementation does not allow coalesced memory access. The new implementation does not only perform coalesced memory access, but also allows to check if all the atom pairs in an entire tile are non-interacting by warp-level primitives, and skip the non-interacting tiles easily.

To ensure the correctness, this PR also adds a new test of `selfcoordnum` with a large atom group "Protein" that has 104 atoms, and enable `pairListFrequency 2` to verify the generation and use of pairlist.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `f4b3b12ef759`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.