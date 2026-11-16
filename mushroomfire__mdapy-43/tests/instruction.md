# Issue

**Title:** DislocationAnalysis: Burgers vectors out of crystal slivers, and spatial vectors from the lattice about each line

## Description

Two fixes to the spatial Burgers vectors of `DislocationAnalysis`, both found by an audit of the analysis.

## 1. Lines traced in a sliver of crystal
A line whose first circuit started in a small cluster of the crystal's own structure kept that cluster's frame and orientation. Next to a Lomer–Cottrell lock, for example, that cluster was the single crystal layer inside an extrinsic fault, 21 atoms. Its fitted orientation was poor (determinant 1.024), so the line's spatial vector was off by 0.46 Å, and the lines of the lock did not add up to the vectors put in.

Now a line in a cluster of the crystal's structure with fewer than 100 atoms is moved into the largest crystal cluster related to it, directly or through one cluster, which is where OVITO reports such lines. On the relaxed lock, the sum goes from (−2.96, −2.83, −0.77) to (−2.99, −3.00, −0.01), in units of a/6 along the crystal axes; the vectors put in add up to (−3, −3, 0). Test: `test_the_lines_of_a_lock_add_up_in_space`, which fails without the fix.

## 2. One cluster holding two grains
When the atoms of a boundary bridge two grains, both programs join them into one cluster, whose orientation is then the mean of the two. The spatial vector of a line in one grain was off by half the angle between them.

Now the lattice about each line is fitted by least squares of the bonds against the ideal neighbor vectors, using the atoms of the line's cluster within 7 Å. Where its rotation differs from the cluster's by more than 3°, with at least 10 atoms, the spatial vector is that rotation times the stretch of the cluster times the lattice vector. Elsewhere it is the orientation of the cluster times the lattice vector, exactly as before. Fitted rotations in single crystals stay within 0.7° of the cluster's (2.75° next to a lock), so 3° separates them from mixed clusters.

Tests:
- `test_a_line_in_one_grain_of_a_cluster_of_two`: a 10° bicrystal joined into one cluster, with a dislocation in one grain. The spatial vector is now 0.03° from that grain's lattice; it was 4.8° off.
- `test_a_single_crystal_keeps_the_mean_orientation`.
- On noisy inputs, the outputs are bit-identical to master.

In a relaxed copper polycrystal, the worst Frank residual at junctions joining two grains falls from 1.14 Å to 0.82 Å. The rest comes from a twin transition that both programs assume between unrelated grains, which is a separate issue; the page now says that such junctions need not add up in space.

The full test suite passes (3557). GCC passes with -Wall -Wextra -Wpedantic -Wshadow -Werror, and ThreadSanitizer passes the C++ checks.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `c3f6bf899f0e`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.