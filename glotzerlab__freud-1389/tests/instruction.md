# Issue

**Title:** Add an optimized CellList implementation based on a linear contiguous buffer

## Description

<!-- Provide a general summary of your changes in the Title above. -->

## Description

Add a new cell list nearest neighbor search, which is significantly faster than the previous AABBQuery. Note that this is a fundamentally different architecture from LinkCell, which is extremely slow compared to both alternatives.

#### TODOs [DONE]

- [x] Fix on windows
- [x] Clean up and lint
- [x] Request review
- [x] Neighbor bonds are inverted occasionally?
- [x] Remove knn

### Architecture

This neighbor list is based on a spatially sorted linear memory region, with cells adjacent in the X direction contiguous in memory. We defer construction until the user attempts a query, allowing us to choose the optimal cell width for a given lookup. For `num_nearest` lookups, we estimate the cell width based on the density of the system, with an empirically determined scale factor for performance. Our choice of construction guarantees we generate every necessary ghost particle in a single layer of ghost cells, which is optimal for performance in `r_max` queries. For `num_nearest` queries, no such guarantee is possible so we do not support that mode. Correctly performing a knn query using this code could otherwise degenerate to all_pairs.


### Performance

For performance, I have two benchmarks -- one based on constructing a full neighbor list in python (`cq.toNeighborList()`) and a more representative test based on computing the RDF of a system with a single bin. This latter benchmark aims to test freud's internal use of lists in `NeighborComputeFunctional`, and is what the text outputs of the code measure performance for. Note that generating random systems and computing the RDF itself takes ~25-30% of the runtime of this benchmark, so the % change performance numbers are underestimates. 

Note in the benchmarks below, I test against [vesin](https://github.com/Luthaf/vesin), which is the fastest nearest neighbor library I've found. Note the results are not fully comparable, however, as they do not use the freud `toNeighborList` function that dominates the runtime.

#### OSX M1 Pro (Python 3.13)

Benchmarks for uniform random systems in random, lightly sheared boxes. r_cut=1.5 and rho=0.5.

```
============================================================
PERCENTAGE IMPROVEMENT: RDF (CellQuery vs AABBQuery)
============================================================

N = 1,000 particles:
  Serial:   +44.0% (AABB: 1.122ms -> Cell: 0.628ms)
  Parallel: +42.3% (AABB: 0.338ms -> Cell: 0.195ms)

N = 2,000 particles:
  Serial:   +49.1% (AABB: 2.305ms -> Cell: 1.174ms)
  Parallel: +45.0% (AABB: 0.685ms -> Cell: 0.377ms)

N = 4,000 particles:
  Serial:   +53.1% (AABB: 5.007ms -> Cell: 2.348ms)
  Parallel: +55.1% (AABB: 1.305ms -> Cell: 0.586ms)

N = 8,000 particles:
  Serial:   +53.7% (AABB: 11.093ms -> Cell: 5.133ms)
  Parallel: +48.7% (AABB: 2.672ms -> Cell: 1.371ms)

N = 16,000 particles:
  Serial:   +53.3% (AABB: 23.478ms -> Cell: 10.958ms)
  Parallel: +59.0% (AABB: 5.807ms -> Cell: 2.381ms)

N = 32,000 particles:
  Serial:   +56.5% (AABB: 43.443ms -> Cell: 18.909ms)
  Parallel: +64.2% (AABB: 11.455ms -> Cell: 4.097ms)

Average improvement across all particle counts:
  Serial:   +51.6%
  Parallel: +52.4%
============================================================
```
![out](https://github.com/user-attachments/assets/b728034b-b309-4afb-8281-0b3253f9173b)

#### Purdue Anvil, `-n 8`

```
============================================================
PERCENTAGE IMPROVEMENT: RDF (CellQuery vs AABBQuery)
============================================================

N = 1,000 particles:
  Serial:   +52.6% (AABB: 1.936ms -> Cell: 0.917ms)
  Parallel: +41.2% (AABB: 0.418ms -> Cell: 0.246ms)

N = 2,000 particles:
  Serial:   +53.8% (AABB: 4.228ms -> Cell: 1.953ms)
  Parallel: +45.9% (AABB: 0.891ms -> Cell: 0.481ms)

N = 4,000 particles:
  Serial:   +55.6% (AABB: 8.794ms -> Cell: 3.903ms)
  Parallel: +48.5% (AABB: 1.751ms -> Cell: 0.901ms)

N = 8,000 particles:
  Serial:   +59.7% (AABB: 17.038ms -> Cell: 6.873ms)
  Parallel: +50.6% (AABB: 3.196ms -> Cell: 1.580ms)

N = 16,000 particles:
  Serial:   +61.1% (AABB: 36.066ms -> Cell: 14.044ms)
  Parallel: +60.6% (AABB: 8.019ms -> Cell: 3.158ms)

N = 32,000 particles:
  Serial:   +60.5% (AABB: 78.146ms -> Cell: 30.855ms)
  Parallel: +63.3% (AABB: 18.246ms -> Cell: 6.698ms)

Average improvement across all particle counts:
  Serial:   +57.2%
  Parallel: +51.7%
============================================================
```

### Comments

freud's `toNeighborList` is extremely slow for systems of reasonable size (<100k particles), mainly due to overhead in the tbb parallel loop. This is true more generally, with many parallel loops in freud incurring performance costs for small-ish systems. This is not a surprise, but does indicate an opportunity for more performance in the future -- although  I don't recall where I saw these figures, I have seen commentary that bs_thread_pool (which we use in SPATULA) has much lower overhead for a similar work-stealing paradigm. We do use more of TBB's machinery throughout freud, but it's worth considering.

Secondly, freud's lazy evaluation of neighbors makes evaluation of certain order parameters relatively inefficient. The issue is that we interleave the pair bond calculations (which have a fair amount of branching and indirection) with what are otherwise fairly dense calculations. This is most notable in fast order parameters like nematic and BOOD, but is true to a lesser extent for `environment` and `density` OPs as well.

Note that this cell list is optimized for uniform, dense systems which is a common pattern within the glotzerlab but perhaps not more generally. AABBQuery will be faster for spatially inhomogeneous data, although we avoid common problems with low-density simulations due to the linear layout of our memory. Because we never rebuild neighbor lists in freud, low-occupancy bins can be stored as efficiently as larger ones, and empty bins can be skipped in the spatial sort entirely. 

There is a wide variety of (reasonably) modern literature on neighbor list calculation. GROMACS advocates for a [blocked tree-based neighbor list](https://arxiv.org/pdf/1306.1737v1) similar to the current AABBQuery, with a few extra tweaks for SIMD between the particles themselves. I tested this as well, but the pattern does not fit freud's lazy evaluation well and the performance was not competitive for reasonable particle counts. There is also research on novel neighbor finding methods for (1) spatially inhomogeneous data ([SNN](https://github.com/nla-group/snn/tree/master), an approach I really like, but it degenerates to all pairs in the uniform case so not useful for crystals) and (2) kNN queries, which do not translate as efficiently to ball queries as the current code in the other direction.


## How Has This Been Tested?

Tests extending the existing `NeighborQueryTest` class have been implemented, with a variety of random systems also tested offline. 

## Checklist:
- [x] I have reviewed the [**Contributor Guidelines**](https://github.com/glotzerlab/freud/blob/main/CONTRIBUTING.md).
- [x] I agree with the terms of the [**Freud Contributor Agreement**](https://github.com/glotzerlab/freud/blob/main/ContributorAgreement.md).
- [x] My name is on the [list of contributors](https://github.com/glotzerlab/freud/blob/main/doc/source/reference/credits.rst) in the pull request source branch.
- [ ] I have updated the [Change log](https://github.com/glotzerlab/freud/blob/main/ChangeLog.md).

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `526e4a5b0a77`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.