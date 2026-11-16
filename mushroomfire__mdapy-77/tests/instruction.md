# Issue

**Title:** DislocationAnalysis: hcp of low c/a

## Description

Builds on #76 (and #75), whose commits it contains until those merge.

`DislocationAnalysis(..., low_ca_ratio=True)` finds hcp of a low c/a, as OVITO's option of the same name does. Below a c/a of some 1.15, the 12 nearest neighbors of hcp no longer form its shells, so every atom is "other": at c/a 1.0, OVITO finds 0 % hcp without the option, and so does MDAPY.

**Method.** When the usual common neighbor analysis of an atom of an hcp crystal fails:

1. Its 6 nearest neighbors are split into the two triplets whose centroids lie farthest apart. These are the triangles above and below the atom; the split is the right one for c/a > 0.707.
2. The c axis joins the two centroids; c is their distance, and a is the mean side of the two triangles seen along the axis.
3. The 14 nearest neighbors are stretched along the axis to the ideal c/a, and the 13 nearest of them are classified again, as hcp only.

The option has no effect on other crystals, and `ElasticStrain` keeps its search as it was.

**Results against OVITO 3.17.1** with `low_ca_ratio=True`. Hot crystals (σ = 0.03 Å) at c/a 0.72, 0.75, 0.8, 0.9, 1.0, 1.1, 1.15, 1.3, 1.633 and 1.9 get the same structure atom by atom as OVITO. That is 98.5 % hcp at 0.75 and all at 0.8 and above; at 0.72, near the limit of the split, both give 51 %. Basal screw and edge dislocations at c/a 0.8, 1.0 and 1.1 give the same structures atom by atom, one 1/3<11-20> line each, and the same Burgers vectors in space and lengths per family.

Tests:

- `tests/analysis/test_dislocations_ovito.py`: the comparisons above.
- `tests/analysis/test_dislocations.py`: at c/a 1.0, no hcp atoms and no lines without the option; with it, the screw along z has a Burgers vector of magnitude a along it. With the option, ideal-c/a hcp and fcc give the same results as without.
- C++: the TSan thread checks and the GCC 15 strict compile pass.

All 3888 tests pass locally.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `89170a9f7cab`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.