# Issue

**Title:** dftd3: fix C6 interpolation at large coordination numbers

## Description

### Summary

The C6 interpolation in `src/force/dftd3/dftd3.cu` sums the pair weights
`exp(-4 [(CN_1 - CNref_i)^2 + (CN_2 - CNref_j)^2])` in single precision. When
this sum falls below 1e-30, both force kernels use the C6 of the last reference
pair and set dC6/dCN to zero (lines 163 and 659 at 9ed3a38c).

For the alkali metals, Cu, Ag and Au, the reference CNs lie only at 0 and about
1. Their weight sums fall below the threshold above CN ≈ 5.1 when the partner
atom is close to one of its own references. For pairs involving sp2 carbon, the
fallback selects the last carbon reference, which is sp3, and underestimates C6
by 17–21%. The switch to the fallback also makes the energy discontinuous at
the threshold, while the forces do not contain the corresponding jump.

Grimme's dftd3 sums the weights in double precision and falls back to the
nearest reference pair only when the sum is below 1e-99. simple-dftd3
normalises the weights per atom. Both return the sp2-based value in these cases.

### Changes

Because the pair weight factorises, the weights are normalised separately for
each atom after the squared CN distances are shifted by their minimum. The
nearest reference then contributes a weight of 1, so each normalising sum is at
least 1 and the fallback can be removed.

The normalised expression is algebraically equivalent to the original
pair-weight formula in exact arithmetic. Where the old code did not fall back,
C6 and dC6/dCN agree up to float rounding. The change removes the discontinuity
at the fallback threshold and gives results consistent with simple-dftd3.

The interpolation is moved into one device function shared by both kernels.
Only `dftd3.cu` is modified.

### Validation

The comparisons use `dftd3 pbe 12 6` on top of NEP89 on an RTX 4090. The D3
contribution is E(NEP + D3) − E(NEP), and the reference is simple-dftd3 1.6.0
with the same cutoffs and no three-body term.

| Quantity | master | this PR | simple-dftd3 |
|---|---|---|---|
| Na above graphene (4×4), D3 interaction at h = 2.20 Å (meV) | -32.93 | -59.49 | -59.49 |
| Same system, change between h = 2.210 and 2.215 Å (meV) | -26.31 | +0.17 | +0.17 |
| Graphene on Cu(111), D3 interaction at d = 3.0 Å (meV per C) | -92.52 | -111.75 | -111.75 |
| NVE, 564 atoms (4 × Na13 on graphene), 300 K, 10 ps: largest deviation from linear fit (meV/atom) | 0.19 | 0.014 | — |

On 39 static structures, comprising small cells on the small-box path and 4×4
replicas on the large-box path, the patched code agrees with simple-dftd3
within 5e-4 meV/atom in energy, 5e-7 eV/Å in force and 4e-4 meV/atom in virial.
A host build of the device function reproduces a double-precision evaluation
within 2.4e-6 in C6 and 5.3e-5 in dC6/dCN (both relative to C6) for 20,000
random pairs over all 94 elements and for targeted cases near the reference CNs.
These comparisons were run at ddc6e2d2; at 9ed3a38c the patched file differs
from the tested one only in an include path.

In `tests_regression` (suite `full`, both executables built with
`CFLAGS="-std=c++14 -O3 -arch=sm_89 -DDEBUG"`), master against itself passes
255/255 cases and 25/25 relations, and master against this branch passes
253/255 cases and 25/25 relations. The two failing cases, `dftd3_single_late`
and `dftd3_kspace_one_each`, differ only at rounding level because the C6 sums
are evaluated in a different order: by at most 1.4e-9 in potential energy and
5.4e-7 in stress (relative), and by at most 5e-12 Å in positions.

With the patch, NEP + D3 on 9024 atoms runs at 2.96e6 atom·step/s, compared with 3.62e6 on master (NEP alone: 1.29e7), an 18% reduction in throughput.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `9ed3a38c2914`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.